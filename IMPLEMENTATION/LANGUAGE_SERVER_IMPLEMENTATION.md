# Part 1. Architecture

---

# 1. Introduction

Language Server는 VSkript Database의 편집기 지원 계층(Editor Integration Layer)이다.

Language Server는 Registry와 Diagnostic 정보를 기반으로 Language Server Protocol(LSP) 요청을 처리하며, 코드 편집 과정에서 실시간 기능을 제공한다.

Language Server는 데이터를 생성하거나 수정하지 않는다.

---

# 2. Responsibilities

Language Server의 주요 책임은 다음과 같다.

- LSP Request 처리
- Registry 조회
- Diagnostic 제공
- 코드 탐색
- 코드 완성
- Hover 정보 제공
- Symbol 검색
- Code Action 제공

---

# 3. Non Goals

Language Server는 다음 작업을 수행하지 않는다.

- Repository Parsing
- Resource Build
- Validation 수행
- Artifact Generation
- Registry 수정

이러한 작업은 각각 Builder, Validator, Generator가 담당한다.

---

# 4. Design Principles

Language Server는 다음 원칙을 따른다.

- Stateless Request Processing
- Immutable Registry Access
- Incremental Document Synchronization
- Deterministic Responses
- Low Latency
- Cancellation Support

---

# 5. High-Level Architecture

```
                Editor
                   │
                   ▼
        Language Server Protocol
                   │
                   ▼
           Request Dispatcher
                   │
                   ▼
            Request Planner
                   │
          ┌────────┼────────┐
          ▼        ▼        ▼
 Completion  Hover  Definition ...
          │        │        │
          └────────┼────────┘
                   ▼
             Registry Query
                   │
                   ▼
               Registry
```

Language Server는 Registry만 조회한다.

---

# 6. Component Overview

Language Server는 다음 컴포넌트로 구성된다.

```
Language Server

├── Request Dispatcher
├── Request Planner
├── Provider Registry
├── Document Manager
├── Workspace Manager
├── Response Builder
├── Diagnostic Manager
└── Telemetry
```

각 컴포넌트는 독립적으로 테스트 가능해야 한다.

---

# 7. Request Dispatcher

Request Dispatcher는 모든 LSP 요청의 진입점이다.

역할

- Request 수신
- Request 분류
- Planner 호출
- Response 반환

Dispatcher는 비즈니스 로직을 포함하지 않는다.

---

# 8. Request Planner

Request Planner는 요청을 적절한 Provider로 라우팅한다.

```
Request

↓

Planner

↓

Provider
```

Planner는 요청 타입과 현재 Document 상태를 고려하여 실행 계획을 수립한다.

---

# 9. Provider Registry

Provider Registry는 모든 LSP Provider를 관리한다.

예시

```
Completion Provider

Hover Provider

Definition Provider

Reference Provider

Rename Provider
```

Provider는 실행 중 동적으로 등록할 수 있다.

---

# 10. Document Manager

Document Manager는 열린 문서를 관리한다.

관리 정보

- URI
- Version
- Language ID
- Content
- Parse State

Document는 Registry와 별도로 관리된다.

---

# 11. Workspace Manager

Workspace Manager는 작업 공간 정보를 관리한다.

예시

- Workspace Root
- Open Projects
- Configuration
- File Watchers

Workspace 변경은 관련 Provider에 통지된다.

---

# 12. Diagnostic Manager

Diagnostic Manager는 Validator에서 생성된 Diagnostic을 관리하고 편집기에 전달한다.

Diagnostic은 Registry가 아니라 별도의 관리 계층을 통해 제공된다.

---

# 13. Response Builder

Response Builder는 Provider의 결과를 LSP 응답 형식으로 변환한다.

```
Provider Result

↓

Response Builder

↓

LSP Response
```

모든 응답은 LSP 명세를 준수해야 한다.

---

# 14. Lifecycle

Language Server의 생명주기는 다음과 같다.

```
Initialize

↓

Load Workspace

↓

Open Documents

↓

Handle Requests

↓

Shutdown

↓

Dispose
```

각 단계는 명확히 분리되어야 한다.

---

# 15. Part Summary

Part 1에서는 Language Server의 전체 아키텍처를 정의하였다.

Language Server는 Request Dispatcher와 Request Planner를 중심으로 동작하며, Registry를 조회하여 편집기 기능을 제공한다.

Part 2에서는 다음 내용을 다룬다.

- Completion Provider
- Hover Provider
- Definition Provider
- Reference Provider
- Rename Provider
- Semantic Tokens Provider
- Document Symbol Provider
- Workspace Symbol Provider

---

# Part 2. Provider Architecture

---

# 16. Overview

Provider는 Language Server의 기능 단위(Function Unit)이다.

각 Provider는 하나의 LSP 기능만 담당하며, Registry와 Diagnostic을 조회하여 결과를 생성한다.

Provider는 서로 독립적으로 구현되어야 한다.

---

# 17. Provider Pipeline

모든 Provider는 동일한 실행 구조를 따른다.

```
LSP Request

↓

Request Dispatcher

↓

Request Planner

↓

Execution Context

↓

Provider

↓

Registry Query

↓

Provider Result

↓

Response Builder

↓

LSP Response
```

Provider는 Registry를 수정해서는 안 된다.

---

# 18. Execution Context

모든 Provider는 Execution Context를 입력으로 사용한다.

Execution Context는 요청 처리에 필요한 모든 정보를 포함한다.

```
Execution Context

├── Request
├── Document
├── Position
├── Workspace
├── Registry Snapshot
├── Diagnostic Snapshot
├── Configuration
├── Cancellation Token
├── Logger
├── Metrics
└── Services
```

Execution Context는 요청 처리 중 변경되지 않는다.

---

# 19. Provider Interface

권장 인터페이스

```typescript
interface LanguageProvider<TResult> {

    provide(
        context: ExecutionContext
    ): Promise<TResult>;

}
```

모든 Provider는 동일한 인터페이스를 따른다.

---

# 20. Completion Provider

Completion Provider는 자동 완성 항목을 생성한다.

조회 대상

- Symbols
- Resources
- Keywords
- Types
- Plugins

Completion 결과는 우선순위에 따라 정렬한다.

---

# 21. Hover Provider

Hover Provider는 현재 심볼의 정보를 제공한다.

포함 정보

- 이름
- 설명
- 타입
- 선언 위치
- 문서 링크

Hover는 Registry Snapshot을 기반으로 생성한다.

---

# 22. Definition Provider

Definition Provider는 심볼의 정의 위치를 반환한다.

지원 대상

- Resource
- Function
- Event
- Type
- Property

정의가 존재하지 않으면 빈 결과를 반환한다.

---

# 23. Reference Provider

Reference Provider는 심볼의 모든 참조를 조회한다.

조회는 Reverse Index를 활용하는 것을 권장한다.

결과는 URI와 위치 정보를 포함한다.

---

# 24. Rename Provider

Rename Provider는 심볼 이름 변경을 지원한다.

절차

```
Validate

↓

Find References

↓

Generate Workspace Edit

↓

Return Result
```

Rename는 Validation 실패 시 중단한다.

---

# 25. Document Symbol Provider

현재 문서의 Symbol Tree를 생성한다.

예시

```
Document

├── Event
├── Function
├── Section
└── Variable
```

결과는 계층 구조를 유지한다.

---

# 26. Workspace Symbol Provider

Workspace 전체에서 Symbol을 검색한다.

검색 대상

- Name
- Namespace
- Plugin
- Type

검색은 Index 기반으로 수행한다.

---

# 27. Semantic Tokens Provider

Semantic Tokens Provider는 구문 강조 정보를 생성한다.

지원 예시

- Type
- Function
- Variable
- Property
- Keyword

Semantic Token은 LSP 명세를 따른다.

---

# 28. Signature Help Provider

함수 및 표현식의 시그니처 정보를 제공한다.

포함 정보

- Parameter 목록
- 현재 Parameter
- Documentation

Signature Help는 Completion과 독립적으로 동작한다.

---

# 29. Code Action Provider

Diagnostic을 기반으로 수정 제안을 생성한다.

예시

- Import 추가
- Rename 제안
- Deprecated API 변경
- 자동 수정

Code Action은 Registry를 직접 수정하지 않는다.

---

# 30. Inlay Hint Provider

코드 이해를 돕기 위한 힌트를 제공한다.

예시

- 타입 힌트
- 매개변수 이름
- 반환 타입

Inlay Hint는 선택 기능이다.

---

# 31. Provider Registry

모든 Provider는 Provider Registry에 등록된다.

```
Provider Registry

├── Completion
├── Hover
├── Definition
├── References
├── Rename
├── Semantic Tokens
├── Code Actions
└── Inlay Hints
```

Request Planner는 Provider Registry를 통해 Provider를 선택한다.

---

# 32. Provider Composition

복합 기능은 여러 Provider를 조합할 수 있다.

예시

```
Rename

↓

Definition Provider

↓

Reference Provider

↓

Workspace Edit
```

Provider 간 직접 의존은 최소화한다.

---

# 33. Provider Lifecycle

Provider는 다음 생명주기를 가진다.

```
Register

↓

Initialize

↓

Handle Request

↓

Dispose
```

Provider는 상태를 유지하지 않는 것을 권장한다.

---

# 34. Provider Error Handling

Provider는 오류를 명확히 구분한다.

- Invalid Request
- Cancelled Request
- Registry Lookup Failure
- Internal Error

예외 대신 명시적인 Result 반환을 권장한다.

---

# 35. Part Summary

Part 2에서는 Language Server의 Provider 아키텍처를 정의하였다.

모든 Provider는 Execution Context를 기반으로 동작하며, Request Planner와 Provider Registry를 통해 요청을 처리한다.

Provider는 Registry를 조회하여 결과를 생성하며, 데이터 변경은 수행하지 않는다.

Part 3에서는 다음 내용을 다룬다.

- Request Dispatcher
- Request Planner
- Execution Pipeline
- Cancellation
- Scheduling
- Concurrency
- Response Builder

---

# Part 3. Request Processing Pipeline

---

# 36. Overview

Language Server는 요청(Request)을 처리하는 엔진이다.

모든 요청은 동일한 Pipeline을 통과한다.

```
Editor

↓

LSP Request

↓

Request Dispatcher

↓

Request Planner

↓

Execution Context

↓

Provider

↓

Registry Query

↓

Response Builder

↓

LSP Response
```

Pipeline은 모든 Request에서 동일하다.

---

# 37. Request Dispatcher

Request Dispatcher는 Language Server의 단일 진입점이다.

역할

- Request 수신
- Request 분류
- Planner 호출
- Cancellation 등록
- Response 반환

Dispatcher는 Provider를 직접 호출하지 않는다.

---

# 38. Request Planner

Planner는 Request를 분석하여 실행 계획을 생성한다.

입력

- Request Type
- Execution Context
- Provider Registry

출력

- Execution Plan

Planner는 Registry를 직접 조회하지 않는다.

---

# 39. Execution Plan

Execution Plan은 Provider 실행 순서를 정의한다.

예시

```
Rename

↓

Definition Provider

↓

Reference Provider

↓

Workspace Edit Builder
```

단순 요청은 하나의 Provider만 실행한다.

---

# 40. Provider Execution

Planner는 Execution Plan에 따라 Provider를 실행한다.

Provider는 독립적으로 실행된다.

동일 Provider는 요청 간 상태를 공유하지 않는다.

---

# 41. Registry Access

Provider는 Registry를 직접 탐색하지 않는다.

항상 Query API를 사용한다.

```
Provider

↓

Registry Query

↓

Lookup Engine

↓

Registry
```

Registry Snapshot은 ExecutionContext에서 제공된다.

---

# 42. Response Builder

Response Builder는 Provider 결과를 LSP 응답으로 변환한다.

역할

- DTO 변환
- Range 계산
- URI 생성
- Error Mapping

모든 응답은 LSP Specification을 준수한다.

---

# 43. Cancellation

모든 Request는 Cancellation을 지원해야 한다.

```
Request

↓

Cancellation Token

↓

Provider

↓

Cancelled
```

Cancellation 이후 추가 작업을 수행해서는 안 된다.

---

# 44. Request Scheduling

Language Server는 요청 우선순위를 가질 수 있다.

권장 우선순위

1. Completion
2. Hover
3. Diagnostics
4. Semantic Tokens
5. Workspace Search

낮은 우선순위 요청은 연기될 수 있다.

---

# 45. Concurrency

독립적인 Request는 병렬 처리할 수 있다.

예시

```
Hover

Completion

Semantic Tokens
```

동일 Document에 대한 충돌은 Scheduler가 조정한다.

---

# 46. Incremental Synchronization

Document 변경은 전체 Workspace를 다시 처리하지 않는다.

```
Text Change

↓

Affected Region

↓

Incremental Update

↓

Providers
```

Incremental Sync를 기본 전략으로 한다.

---

# 47. Document Versioning

모든 요청은 특정 Document Version을 기준으로 수행한다.

Version이 변경되면 이전 요청 결과는 폐기할 수 있다.

---

# 48. Error Recovery

Recoverable Error

- Provider Failure
- Lookup Failure
- Missing Symbol

Fatal Error

- Invalid Registry
- Corrupted Snapshot

Recoverable Error는 다른 Provider 실행을 방해하지 않는다.

---

# 49. Metrics

Language Server는 다음 정보를 수집한다.

- Request Count
- Average Latency
- Provider Time
- Cancellation Count
- Cache Hit Ratio

Metrics는 Telemetry에 전달할 수 있다.

---

# 50. Part Summary

Part 3에서는 Request Processing Pipeline을 정의하였다.

Language Server는 Request Dispatcher와 Request Planner를 중심으로 실행되며, Registry Query와 Response Builder를 통해 결정적이고 예측 가능한 LSP 응답을 생성한다.

Part 4에서는 다음 내용을 다룬다.

- Provider Cache
- Incremental Cache
- Workspace Cache
- Performance
- Memory Model
- Scalability

---

# Part 4. Performance, Caching & Scalability

---

# 51. Overview

Language Server는 사용자의 입력에 즉시 반응해야 한다.

낮은 지연 시간(Low Latency)을 유지하기 위해 다음 전략을 사용한다.

- Incremental Synchronization
- Multi-Level Cache
- Parallel Request Processing
- Lazy Computation
- Snapshot Isolation

---

# 52. Performance Goals

권장 목표

| Metric | Target |
|---------|--------:|
| Completion | < 20 ms |
| Hover | < 20 ms |
| Go to Definition | < 30 ms |
| References | < 50 ms |
| Rename | < 100 ms |
| Semantic Tokens | < 200 ms |

목표치는 일반적인 프로젝트 규모를 기준으로 한다.

---

# 53. Cache Architecture

Language Server는 계층형(Cache Hierarchy)을 사용한다.

```
Language Server

├── Document Cache
├── Symbol Cache
├── Query Cache
├── Provider Cache
└── Workspace Cache
```

각 Cache는 독립적으로 관리된다.

---

# 54. Document Cache

Document Cache는 열린 문서의 상태를 저장한다.

포함 정보

- URI
- Version
- Parse Tree
- Tokens

Document Version이 변경되면 해당 Cache를 갱신한다.

---

# 55. Symbol Cache

Symbol Cache는 자주 조회되는 Symbol 정보를 저장한다.

예시

- Completion Symbols
- Hover Symbols
- Workspace Symbols

Symbol Cache는 Registry Snapshot에 종속된다.

---

# 56. Query Cache

동일한 Query는 캐시된 결과를 재사용할 수 있다.

Cache Key

- Query Type
- Parameters
- Registry Version
- Document Version

Cache는 Snapshot 단위로 관리한다.

---

# 57. Provider Cache

Provider는 계산 비용이 큰 결과를 캐시할 수 있다.

예시

- Semantic Tokens
- Document Symbols
- Folding Ranges
- Inlay Hints

Provider Cache는 Provider 내부 구현에 종속되지 않아야 한다.

---

# 58. Workspace Cache

Workspace 전체를 대상으로 하는 결과를 저장한다.

예시

- Workspace Symbols
- File Index
- Namespace Index

Workspace 변경 시 영향을 받는 영역만 갱신한다.

---

# 59. Cache Invalidation

다음 경우 Cache를 무효화한다.

- Document Version 변경
- Registry Snapshot 변경
- Workspace 변경
- Configuration 변경

가능한 한 부분 무효화(Partial Invalidation)를 수행한다.

---

# 60. Incremental Synchronization

Language Server는 변경된 영역만 다시 처리한다.

```
Document Change

↓

Affected Region

↓

Incremental Analysis

↓

Updated Cache
```

전체 문서를 다시 분석하는 것은 예외적인 경우에만 허용한다.

---

# 61. Lazy Evaluation

비용이 큰 계산은 필요할 때 수행한다.

예시

- References
- Workspace Search
- Semantic Tokens

Lazy Evaluation은 응답 지연을 최소화하는 것을 목표로 한다.

---

# 62. Parallel Execution

독립적인 Provider는 병렬 실행할 수 있다.

예시

```
Hover

Completion

Document Symbols
```

공유 상태는 Registry Snapshot을 통해 제공한다.

---

# 63. Memory Strategy

메모리 사용 최소화를 위해 다음을 권장한다.

- Immutable Snapshot 공유
- Object Reuse
- Lazy Materialization
- Partial Cache

중복 객체 생성을 최소화한다.

---

# 64. Scalability

Language Server는 대규모 Workspace를 지원해야 한다.

확장 대상

- Multi-root Workspace
- Large Repository
- Multiple Plugins
- Thousands of Symbols

성능은 프로젝트 크기에 비례하여 완만하게 증가하는 것을 목표로 한다.

---

# 65. Telemetry

선택적으로 다음 정보를 수집할 수 있다.

- Request Latency
- Cache Hit Ratio
- Provider Execution Time
- Cancellation Count
- Error Count

Telemetry는 기능 개선을 위한 통계 목적으로만 사용한다.

---

# 66. Resource Management

Language Server는 사용하지 않는 리소스를 해제해야 한다.

대상

- Closed Documents
- Expired Cache
- Stale Workspace Entries

리소스 정리는 백그라운드 작업으로 수행할 수 있다.

---

# 67. Failure Handling

성능 최적화 기능이 실패하더라도 기본 기능은 유지되어야 한다.

예시

- Cache 실패 → 직접 계산
- Lazy Evaluation 실패 → 즉시 계산

최적화는 기능의 정확성을 저해해서는 안 된다.

---

# 68. Best Practices

권장 사항

- Incremental Sync를 기본으로 사용한다.
- Cache를 Snapshot 단위로 관리한다.
- Registry를 직접 탐색하지 않는다.
- 장시간 작업은 Cancellation을 지원한다.
- Provider는 Stateless를 유지한다.

---

# 69. Part Summary

Part 4에서는 Language Server의 성능 모델을 정의하였다.

Language Server는 Incremental Synchronization과 계층형 Cache를 기반으로 낮은 지연 시간을 제공하며, Registry Snapshot을 공유하여 병렬 처리와 확장성을 확보한다.

Part 5에서는 다음 내용을 다룬다.

- Extension Model
- Custom Providers
- Testing
- Compliance
- Design Principles
- Document Summary

---

# Part 4. Performance, Caching & Scalability

---

# 51. Overview

Language Server는 사용자의 입력에 즉시 반응해야 한다.

낮은 지연 시간(Low Latency)을 유지하기 위해 다음 전략을 사용한다.

- Incremental Synchronization
- Multi-Level Cache
- Parallel Request Processing
- Lazy Computation
- Snapshot Isolation

---

# 52. Performance Goals

권장 목표

| Metric | Target |
|---------|--------:|
| Completion | < 20 ms |
| Hover | < 20 ms |
| Go to Definition | < 30 ms |
| References | < 50 ms |
| Rename | < 100 ms |
| Semantic Tokens | < 200 ms |

목표치는 일반적인 프로젝트 규모를 기준으로 한다.

---

# 53. Cache Architecture

Language Server는 계층형(Cache Hierarchy)을 사용한다.

```
Language Server

├── Document Cache
├── Symbol Cache
├── Query Cache
├── Provider Cache
└── Workspace Cache
```

각 Cache는 독립적으로 관리된다.

---

# 54. Document Cache

Document Cache는 열린 문서의 상태를 저장한다.

포함 정보

- URI
- Version
- Parse Tree
- Tokens

Document Version이 변경되면 해당 Cache를 갱신한다.

---

# 55. Symbol Cache

Symbol Cache는 자주 조회되는 Symbol 정보를 저장한다.

예시

- Completion Symbols
- Hover Symbols
- Workspace Symbols

Symbol Cache는 Registry Snapshot에 종속된다.

---

# 56. Query Cache

동일한 Query는 캐시된 결과를 재사용할 수 있다.

Cache Key

- Query Type
- Parameters
- Registry Version
- Document Version

Cache는 Snapshot 단위로 관리한다.

---

# 57. Provider Cache

Provider는 계산 비용이 큰 결과를 캐시할 수 있다.

예시

- Semantic Tokens
- Document Symbols
- Folding Ranges
- Inlay Hints

Provider Cache는 Provider 내부 구현에 종속되지 않아야 한다.

---

# 58. Workspace Cache

Workspace 전체를 대상으로 하는 결과를 저장한다.

예시

- Workspace Symbols
- File Index
- Namespace Index

Workspace 변경 시 영향을 받는 영역만 갱신한다.

---

# 59. Cache Invalidation

다음 경우 Cache를 무효화한다.

- Document Version 변경
- Registry Snapshot 변경
- Workspace 변경
- Configuration 변경

가능한 한 부분 무효화(Partial Invalidation)를 수행한다.

---

# 60. Incremental Synchronization

Language Server는 변경된 영역만 다시 처리한다.

```
Document Change

↓

Affected Region

↓

Incremental Analysis

↓

Updated Cache
```

전체 문서를 다시 분석하는 것은 예외적인 경우에만 허용한다.

---

# 61. Lazy Evaluation

비용이 큰 계산은 필요할 때 수행한다.

예시

- References
- Workspace Search
- Semantic Tokens

Lazy Evaluation은 응답 지연을 최소화하는 것을 목표로 한다.

---

# 62. Parallel Execution

독립적인 Provider는 병렬 실행할 수 있다.

예시

```
Hover

Completion

Document Symbols
```

공유 상태는 Registry Snapshot을 통해 제공한다.

---

# 63. Memory Strategy

메모리 사용 최소화를 위해 다음을 권장한다.

- Immutable Snapshot 공유
- Object Reuse
- Lazy Materialization
- Partial Cache

중복 객체 생성을 최소화한다.

---

# 64. Scalability

Language Server는 대규모 Workspace를 지원해야 한다.

확장 대상

- Multi-root Workspace
- Large Repository
- Multiple Plugins
- Thousands of Symbols

성능은 프로젝트 크기에 비례하여 완만하게 증가하는 것을 목표로 한다.

---

# 65. Telemetry

선택적으로 다음 정보를 수집할 수 있다.

- Request Latency
- Cache Hit Ratio
- Provider Execution Time
- Cancellation Count
- Error Count

Telemetry는 기능 개선을 위한 통계 목적으로만 사용한다.

---

# 66. Resource Management

Language Server는 사용하지 않는 리소스를 해제해야 한다.

대상

- Closed Documents
- Expired Cache
- Stale Workspace Entries

리소스 정리는 백그라운드 작업으로 수행할 수 있다.

---

# 67. Failure Handling

성능 최적화 기능이 실패하더라도 기본 기능은 유지되어야 한다.

예시

- Cache 실패 → 직접 계산
- Lazy Evaluation 실패 → 즉시 계산

최적화는 기능의 정확성을 저해해서는 안 된다.

---

# 68. Best Practices

권장 사항

- Incremental Sync를 기본으로 사용한다.
- Cache를 Snapshot 단위로 관리한다.
- Registry를 직접 탐색하지 않는다.
- 장시간 작업은 Cancellation을 지원한다.
- Provider는 Stateless를 유지한다.

---

# 69. Part Summary

Part 4에서는 Language Server의 성능 모델을 정의하였다.

Language Server는 Incremental Synchronization과 계층형 Cache를 기반으로 낮은 지연 시간을 제공하며, Registry Snapshot을 공유하여 병렬 처리와 확장성을 확보한다.

Part 5에서는 다음 내용을 다룬다.

- Extension Model
- Custom Providers
- Testing
- Compliance
- Design Principles
- Document Summary

---

# Part 5. Extension, Testing & Compliance

---

# 70. Overview

Language Server는 장기간 실행되는 서비스이며 다양한 Provider와 기능 확장을 지원해야 한다.

본 장에서는 Language Server의 확장 모델, 테스트 전략, 구현 준수 사항 및 설계 원칙을 정의한다.

---

# 71. Extension Model

Language Server는 새로운 기능을 기존 구현을 수정하지 않고 확장할 수 있어야 한다.

확장은 다음 계층에서 이루어진다.

```
Language Server

↓

Extension

↓

Provider

↓

Registry
```

새로운 기능은 기존 Provider와 독립적으로 등록할 수 있어야 한다.

---

# 72. Custom Providers

프로젝트는 사용자 정의 Provider를 지원할 수 있다.

예시

- Completion Provider
- Hover Provider
- Definition Provider
- Reference Provider
- Code Action Provider
- Semantic Token Provider

모든 Provider는 공통 인터페이스를 구현해야 한다.

---

# 73. Provider Registration

Provider 등록은 Registry를 통해 수행한다.

```
Provider

↓

Registry

↓

Capability Map

↓

Language Server
```

동일 Capability에 여러 Provider가 존재할 경우 우선순위 정책을 명시해야 한다.

---

# 74. Capability Compliance

모든 Provider는 선언한 Capability만 구현해야 한다.

예시

```
Completion Provider

↓

completion
```

Hover Provider가 Completion 요청을 처리해서는 안 된다.

Capability와 구현은 항상 일치해야 한다.

---

# 75. Testing Strategy

Language Server는 다음 수준의 테스트를 수행하는 것을 권장한다.

- Unit Test
- Integration Test
- Protocol Test
- Workspace Test
- Performance Test

테스트는 가능한 한 실제 LSP 요청을 기반으로 구성한다.

---

# 76. Mock Components

테스트 환경에서는 다음 구성 요소를 Mock으로 대체할 수 있다.

- Registry
- Workspace
- Configuration
- Parser
- File System
- Logger

Mock은 실제 인터페이스를 유지해야 한다.

---

# 77. Compliance Requirements

Language Server 구현은 다음 요구사항을 만족해야 한다.

- Context Model 준수
- Graph Model 준수
- Registry Model 준수
- Snapshot 기반 동작
- Immutable Context 사용
- Stable Ordering 유지

모든 구현은 프로젝트 Core Specification과 일관성을 유지해야 한다.

---

# 78. Error Handling

Provider 오류는 Language Server 전체를 중단시켜서는 안 된다.

권장 처리 방식

```
Provider Error

↓

Diagnostic

↓

Log

↓

Continue Processing
```

복구 가능한 오류는 가능한 한 요청 단위에서 처리한다.

---

# 79. Design Principles

Language Server는 다음 원칙을 따른다.

- Context-driven Execution
- Registry as Single Source of Truth
- Immutable Snapshots
- Incremental Processing
- Explicit Capabilities
- Provider Isolation
- Deterministic Results

Language Server는 실행 환경을 조율하는 역할을 수행하며, 개별 비즈니스 로직은 Provider에 위임한다.

---

# 80. Reference Architecture

Language Server의 참조 구조는 다음과 같다.

```
Client

↓

Protocol Layer

↓

Request Dispatcher

↓

Execution Context

↓

Provider Registry

↓

Providers

↓

Registry Snapshot

↓

Response Builder
```

모든 요청은 동일한 실행 흐름을 따른다.

---

# 81. Cross References

본 문서는 다음 문서와 함께 사용한다.

- SPEC-0013 Context Model
- SPEC-0014 Graph Model
- RFC-0007 Builder
- RFC-0008 Validator
- RFC-0009 Generator
- ADR-0004 Registry as Single Source of Truth

---

# 82. Document Summary

Language Server는 VSkript Database에서 Language Server Protocol 요청을 처리하는 핵심 실행 컴포넌트이다.

모든 요청은 Execution Context를 중심으로 처리되며, Provider Registry를 통해 적절한 Provider로 위임된다.

Language Server는 Snapshot 기반 Registry를 참조하여 일관된 결과를 제공하며, 확장 가능한 Provider 구조를 통해 새로운 기능을 안전하게 추가할 수 있다.

---

# 83. Revision History

| Version | Description |
|----------|-------------|
| 1.0 | Initial Language Server Implementation Guide |

