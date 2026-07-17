# BUILDER_IMPLEMENTATION.md

Version: 1.0

Status: Draft

Related Documents

- RFC-0007 Builder Specification
- ADR-0003 Incremental Build Architecture
- ADR-0004 Registry as Source of Truth
- SPEC-0010 Dependency Resolution
- SPEC-0011 DataPack Layout

---

# Part 1. Architecture & Build Pipeline

---

# 1. Introduction

Builder는 VSkript Database의 첫 번째 실행 단계이다.

Repository에 존재하는 Raw Resource를 읽고,
이를 내부 Build Model로 변환하여
Validator와 Generator가 사용할 수 있는 Build Context를 생성한다.

Builder는 Validation이나 Packaging을 수행하지 않는다.

Builder의 결과는 항상 Deterministic 해야 한다.

동일한 입력은 동일한 Build Output을 생성해야 한다.

---

# 2. Responsibilities

Builder의 책임은 다음과 같다.

- Resource Discovery
- Resource Loading
- JSON Parsing
- Schema Resolution
- Dependency Resolution
- Registry Generation
- Build Context 생성
- Incremental Build 처리

Builder는 다음 작업을 수행하지 않는다.

- Validation
- Package 생성
- ZIP 생성
- Marketplace Upload
- Language Server 처리

---

# 3. Design Principles

Builder는 다음 원칙을 따른다.

## Deterministic

동일 입력 → 동일 출력

Build 결과는 실행 순서와 무관해야 한다.

---

## Stateless

Build는 이전 실행 상태에 의존하지 않는다.

Incremental Build에서 사용하는 Cache 역시 재생성 가능해야 한다.

---

## Immutable Input

Repository의 원본 데이터를 수정하지 않는다.

Builder는 읽기 전용이다.

---

## Fail Fast

치명적인 오류는 가능한 한 빠르게 발견한다.

예시

- JSON Parse Error
- Missing Manifest
- Duplicate Resource ID

---

## Extensible

새로운 Resource Type 추가 시

Builder Core를 수정하지 않고
Plugin Loader만 추가 가능해야 한다.

---

# 4. High Level Architecture

```
                 Repository
                      │
          ┌───────────┴───────────┐
          │                       │
     Resource Loader        Manifest Loader
          │                       │
          └───────────┬───────────┘
                      ▼
                 Resource Parser
                      │
                      ▼
              Dependency Resolver
                      │
                      ▼
              Registry Generator
                      │
                      ▼
                Build Context
                      │
         ┌────────────┴────────────┐
         ▼                         ▼
    Validator                 Generator
```

Builder는 Build Context까지만 생성한다.

---

# 5. Build Pipeline

Builder Pipeline은 다음 순서로 수행한다.

```
Discover

↓

Load

↓

Parse

↓

Resolve Schema

↓

Resolve Dependency

↓

Generate Registry

↓

Generate Build Context

↓

Finish
```

모든 단계는 독립적으로 테스트 가능해야 한다.

---

# 6. Build Context

Builder의 최종 출력은 Build Context이다.

예시

```text
BuildContext

├── Manifest
├── Registry
├── Resource Graph
├── Dependency Graph
├── Build Configuration
├── Diagnostics
└── Metadata
```

Validator와 Generator는 Build Context만 사용한다.

Repository를 다시 탐색해서는 안 된다.

---

# 7. Repository Discovery

Builder는 Repository를 탐색하여 Build 대상을 수집한다.

Discovery 대상

- Plugins
- Grammar
- Locale
- Types
- Metadata
- Assets
- Documentation

Discovery는 Manifest를 기준으로 수행한다.

Directory Traversal만으로 Plugin을 찾지 않는다.

---

# 8. Resource Loader

Loader의 역할은 파일을 메모리로 읽는 것이다.

Loader는

- Parsing
- Validation

을 수행하지 않는다.

출력

```text
ResourceFile

path

contents

lastModified

hash
```

---

# 9. Resource Parser

Parser는 Raw JSON을 내부 모델로 변환한다.

예시

```
effects/spawn.json

↓

GrammarModel
```

Parser는 JSON Syntax Error만 검사한다.

Schema Validation은 Validator의 책임이다.

---

# 10. Internal Build Model

Builder는 Repository 구조를 그대로 사용하지 않는다.

모든 Resource는 내부 모델로 변환된다.

예시

```text
GrammarResource

id

plugin

namespace

category

dependencies

sourcePath
```

Validator와 Generator는 내부 모델만 사용한다.

---

# 11. Build Configuration

Builder는 다음 설정을 지원한다.

```yaml
mode: full

cache: true

parallel: true

workers: auto

generateRegistry: true

incremental: true
```

Configuration은 Build 시작 전에 고정된다.

---

# 12. Build Lifecycle

Builder의 전체 생명주기

```
Initialize

↓

Load Configuration

↓

Discover Resources

↓

Load Files

↓

Parse

↓

Resolve

↓

Generate Registry

↓

Build Context

↓

Complete
```

모든 단계는 로그로 기록되어야 한다.

---

# 13. Error Handling

Builder는 다음 오류를 구분한다.

Fatal

- Missing Manifest
- Invalid Repository
- Circular Dependency
- Duplicate Resource ID

Recoverable

- Missing Documentation
- Empty Locale
- Missing Optional Metadata

Fatal Error 발생 시 Build를 중단한다.

---

# 14. Logging

Builder는 구조화된 로그를 출력한다.

예시

```
INFO

Loading Plugin Skript
```

```
INFO

Loaded 126 Grammar Files
```

```
WARN

Locale missing: ko_kr
```

```
ERROR

Duplicate Resource ID
```

모든 로그는 Plugin 이름을 포함해야 한다.

---

# 15. Extension Points

Builder는 다음 확장을 지원한다.

- New Resource Loader
- New Parser
- New Registry Generator
- New Discovery Strategy
- New Build Hook

Core Builder 수정 없이 확장 가능해야 한다.

---

# Part 1 Summary

Part 1에서는 Builder의 전체 구조와 Build Pipeline을 정의하였다.

다음 Part에서는 다음 내용을 다룬다.

- Build Graph
- Dependency Resolution
- Registry Generation
- Build Ordering
- Topological Sort
- Cycle Detection
- Plugin Dependency Graph

---

# Part 2. Build Graph & Dependency Resolution

---

# 16. Build Graph

Builder는 Repository를 단순한 파일 집합으로 취급하지 않는다.

모든 Resource는 Build Graph의 Node가 된다.

```
                 Build Graph

         Grammar
          /    \
         /      \
    Locale     Type
         \      /
          \    /
         Manifest
```

Graph는 Build 순서를 결정하는 핵심 자료구조이다.

---

# 17. Build Graph Node

모든 Resource는 BuildNode로 표현된다.

```text
BuildNode

id

plugin

resourceType

sourcePath

dependencies[]

dependents[]

hash

status
```

Node는 실제 Resource가 아니라 Build 대상이다.

---

# 18. Node Status

Node는 다음 상태를 가진다.

```
DISCOVERED

↓

LOADED

↓

PARSED

↓

RESOLVED

↓

READY

↓

BUILT
```

오류 발생 시

```
FAILED
```

상태로 전환된다.

---

# 19. Edge Definition

Edge는 Dependency를 의미한다.

```
Grammar

↓

Type
```

Grammar는 Type에 의존한다.

따라서

Type이 먼저 Build된다.

---

# 20. Dependency Sources

Dependency는 다음에서 발견된다.

Manifest

Grammar

Locale

Metadata

Registry

Plugin Dependency

Builder는 모든 Dependency를 하나의 Graph로 통합한다.

---

# 21. Dependency Resolution Pipeline

```
Discover Nodes

↓

Collect Dependencies

↓

Resolve IDs

↓

Generate Graph

↓

Detect Cycles

↓

Topological Sort

↓

Build Queue
```

---

# 22. Resource Identifier Resolution

Dependency는 항상 Resource ID로 표현된다.

예시

```
skript.type.player
```

Builder는 Registry를 이용하여 실제 Resource를 찾는다.

직접 Directory를 검색하지 않는다.

---

# 23. Registry Lookup

Lookup 과정

```
Resource ID

↓

Registry

↓

Metadata

↓

Source Path

↓

BuildNode
```

Registry는 Lookup Engine 역할을 한다.

---

# 24. Topological Sorting

Graph는 Build 전에 Topological Sort를 수행한다.

예시

```
Type

↓

Expression

↓

Effect

↓

Event
```

Build 순서는 Dependency를 항상 만족해야 한다.

---

# 25. Topological Sort Algorithm

권장 알고리즘

- Kahn Algorithm

또는

- DFS Topological Sort

Builder는 결정적인 결과를 보장해야 한다.

동일 입력은 동일 Build Order를 생성해야 한다.

---

# 26. Example Build Order

```
Manifest

↓

Types

↓

Conditions

↓

Expressions

↓

Effects

↓

Events

↓

Registry
```

Registry는 항상 마지막에 생성된다.

---

# 27. Cycle Detection

Builder는 Cycle을 허용하지 않는다.

예시

```
Type A

↓

Type B

↓

Type C

↓

Type A
```

Build는 즉시 실패한다.

---

# 28. Cycle Detection Algorithm

권장 알고리즘

DFS

또는

Tarjan SCC

Cycle 발견 시

```
Circular Dependency

A

↓

B

↓

C

↓

A
```

를 출력해야 한다.

---

# 29. Plugin Dependency Graph

Plugin도 Graph를 가진다.

```
SkBee

↓

Skript
```

Builder는 Plugin Graph를 먼저 해결한 뒤

Plugin 내부 Graph를 처리한다.

---

# 30. Cross Plugin Dependency

허용

```
SkBee

↓

Skript Type
```

금지

```
SkBee

↓

Skript Internal File
```

항상 Registry를 통해 접근한다.

---

# 31. Build Queue

Topological Sort 결과는 Build Queue가 된다.

```
Queue

↓

Manifest

↓

Type

↓

Grammar

↓

Registry
```

Queue는 Worker에게 전달된다.

---

# 32. Build Stage

각 Node는 동일한 Stage를 가진다.

```
Load

↓

Parse

↓

Resolve

↓

Ready

↓

Build
```

Worker는 Stage를 순차적으로 수행한다.

---

# 33. Build Barrier

Registry는 모든 Node가 Build된 후 생성된다.

```
All Nodes Built

↓

Barrier

↓

Generate Registry
```

Barrier 이전에는 Registry를 생성하지 않는다.

---

# 34. Build Context Update

Node가 Build될 때마다 Context를 갱신한다.

```
Build Node

↓

Update Context

↓

Continue
```

Validator는 최신 Context를 사용한다.

---

# 35. Error Propagation

Dependency Build 실패 시

하위 Node는 Skip된다.

예시

```
Type

FAILED

↓

Expression

SKIPPED

↓

Effect

SKIPPED
```

연쇄 오류를 방지한다.

---

# 36. Graph Validation

Build 전에 Graph를 검사한다.

검사 항목

- Duplicate Node
- Missing Dependency
- Circular Dependency
- Invalid Namespace
- Invalid Plugin

문제가 있으면 Build를 시작하지 않는다.

---

# 37. Graph Serialization

Build Graph는 디버깅을 위해 저장할 수 있다.

예시

```json
{
  "nodes": [...],
  "edges": [...]
}
```

CI에서는 Artifact로 업로드할 수 있다.

---

# 38. Public Interfaces

```text
IBuildGraph

addNode()

addEdge()

resolve()

topologicalSort()

detectCycles()
```

---

```text
IDependencyResolver

resolve()

validate()

lookup()
```

---

# 39. Pseudocode

```text
discover()

↓

buildNodes()

↓

resolveDependencies()

↓

detectCycles()

↓

topologicalSort()

↓

buildQueue()

↓

execute()
```

---

# 40. Part 2 Summary

Builder는 모든 Resource를 Build Graph로 변환한다.

Dependency Resolution을 수행한 후 Topological Sort를 통해 Build Queue를 생성한다.

이 과정은 Incremental Build, Parallel Build, Registry Generation의 기반이 된다.

Part 3에서는 다음 내용을 다룬다.

- Dirty Resource Detection
- Incremental Build
- Cache System
- Build Snapshot
- Hash Strategy
- Registry Diff
- Build Optimization

---

# Part 3. Incremental Build & Cache

---

# 41. Overview

Incremental Build는 Builder의 가장 중요한 성능 최적화 기능이다.

Builder는 Repository 전체를 다시 Build하지 않는다.

변경된 Resource와 영향을 받는 Resource만 다시 Build한다.

목표

- Build 시간 최소화
- CI/CD 시간 단축
- IDE 개발 생산성 향상
- Cache 재사용 극대화

---

# 42. Incremental Build Pipeline

Incremental Build는 다음 순서로 수행된다.

```
Load Previous Snapshot

↓

Discover Current Resources

↓

Compute Hash

↓

Compare Snapshot

↓

Detect Dirty Resources

↓

Resolve Dependencies

↓

Generate Build Queue

↓

Incremental Build

↓

Update Snapshot
```

Builder는 항상 이전 Build Snapshot과 현재 Repository를 비교한다.

---

# 43. Build Snapshot

Build Snapshot은 이전 Build 상태를 저장한다.

예시

```text
BuildSnapshot

├── Builder Version
├── Schema Version
├── Registry Version
├── Resource Hashes
├── Dependency Graph
├── Registry Snapshot
├── Build Timestamp
└── Build Configuration
```

Snapshot은 Incremental Build의 기준 데이터이다.

---

# 44. Resource Fingerprint

모든 Resource는 Fingerprint(Hash)를 가진다.

예시

```text
SHA256(
    file contents
    + schemaVersion
    + builderVersion
)
```

Hash가 동일하면 Resource는 변경되지 않은 것으로 간주한다.

Timestamp는 변경 여부 판단 기준으로 사용하지 않는다.

---

# 45. Dirty Resource Detection

Dirty Resource는 다음 조건 중 하나를 만족하면 생성된다.

- File Added
- File Removed
- File Modified
- Manifest Changed
- Registry Changed
- Dependency Changed
- Schema Version Changed

Dirty Resource는 Build Queue의 시작점이 된다.

---

# 46. Dirty Propagation

Dirty Resource는 자신에게 의존하는 모든 Resource로 전파된다.

예시

```
Type

↓

Expression

↓

Effect

↓

Event
```

Type이 변경되면

- Expression
- Effect
- Event

모두 Dirty가 된다.

---

# 47. Dependency Impact Analysis

Dirty Detection은 단순히 변경 파일만 찾는 것이 아니다.

Builder는 Dependency Graph를 이용하여 영향을 계산한다.

```
Changed Node

↓

Dependency Graph

↓

Affected Nodes

↓

Dirty Set
```

---

# 48. Dirty Set

Dirty Set은 Incremental Build의 대상이다.

예시

```text
DirtySet

- skript.type.player
- skript.expression.target
- skript.effect.teleport
```

Dirty Set 외의 Resource는 Build하지 않는다.

---

# 49. Clean Resource

Clean Resource는 이전 Build 결과를 그대로 재사용한다.

```
Clean Resource

↓

Skip Build

↓

Reuse Cache
```

Builder는 Clean Resource를 다시 Parsing하지 않는다.

---

# 50. Build Cache

Builder는 Build 결과를 캐시에 저장한다.

저장 대상

- Parsed Model
- AST
- Dependency Result
- Registry Entry
- Build Diagnostics

---

# 51. Cache Architecture

```
             Cache Manager

                    │

      ┌─────────────┼─────────────┐

      ▼             ▼             ▼

 Resource Cache  Graph Cache  Registry Cache
```

각 Cache는 독립적으로 관리된다.

---

# 52. Cache Key

Cache Key는 다음 요소를 조합하여 생성한다.

```text
Resource ID

+

Resource Hash

+

Schema Version

+

Builder Version
```

동일한 Key는 동일한 Build 결과를 의미한다.

---

# 53. Cache Hit

```
Lookup Cache

↓

Found

↓

Reuse Result
```

Builder는 Parsing을 생략한다.

---

# 54. Cache Miss

```
Lookup Cache

↓

Not Found

↓

Load

↓

Parse

↓

Store Cache
```

새로운 Build 결과를 생성한다.

---

# 55. Cache Invalidation

다음 경우 Cache는 반드시 무효화된다.

- Schema Version 변경
- Builder Version 변경
- Resource 변경
- Dependency 변경
- Registry 변경
- Build Configuration 변경

Cache는 부분 무효화를 지원해야 한다.

---

# 56. Registry Diff

Incremental Build는 Registry Diff를 생성한다.

```
Previous Registry

↓

Current Registry

↓

Diff
```

Diff에는 다음이 포함된다.

- Added Resources
- Removed Resources
- Modified Resources

---

# 57. Snapshot Update

Build 완료 후 Snapshot을 갱신한다.

```
Incremental Build

↓

Registry Update

↓

Snapshot Update

↓

Save
```

다음 Build는 이 Snapshot을 기준으로 수행된다.

---

# 58. Build Optimization

Builder는 다음 최적화를 수행할 수 있다.

- Lazy Parsing
- Lazy Registry Update
- Graph Reuse
- Shared Dependency Resolution
- Batch IO

최적화는 Build 결과를 변경해서는 안 된다.

---

# 59. Failure Recovery

Incremental Build가 실패하면

Builder는 다음 중 하나를 수행한다.

```
Retry Incremental
```

또는

```
Fallback Full Build
```

Snapshot이 손상된 경우 Full Build를 수행한다.

---

# 60. Public Interfaces

```text
IIncrementalBuilder

loadSnapshot()

detectDirty()

build()

saveSnapshot()
```

---

```text
ICacheManager

get()

put()

invalidate()

clear()
```

---

```text
ISnapshotStore

load()

save()

delete()
```

---

# 61. Example Flow

```
Build Start

↓

Load Snapshot

↓

Hash Resources

↓

Detect Dirty

↓

Resolve Dependencies

↓

Incremental Queue

↓

Build

↓

Registry Update

↓

Save Snapshot

↓

Done
```

---

# 62. Pseudocode

```text
snapshot = loadSnapshot()

dirty = detectDirty(snapshot)

affected = propagateDependencies(dirty)

queue = buildQueue(affected)

execute(queue)

saveSnapshot()
```

---

# 63. Performance Goals

권장 목표

| Metric | Target |
|----------|---------|
| Dirty Detection | <100ms |
| Cache Lookup | O(1) |
| Registry Diff | O(n) |
| Incremental Build | O(changed nodes) |
| Snapshot Save | <50ms |

Builder는 Repository 크기보다 변경량에 비례하여 동작해야 한다.

---

# 64. Testing Requirements

Incremental Build는 다음 테스트를 포함해야 한다.

- Single File Change
- Multiple File Change
- Manifest Change
- Dependency Change
- Deleted Resource
- Added Resource
- Cache Hit
- Cache Miss
- Snapshot Recovery
- Full Build Fallback

---

# 65. Part 3 Summary

Builder는 Build Snapshot과 Cache를 활용하여 변경된 Resource만 다시 Build한다.

Incremental Build는 Dependency Graph와 Registry Diff를 기반으로 Build 대상을 계산하며, Snapshot과 Cache는 항상 재생성 가능한 데이터로 유지된다.

Part 4에서는 다음 내용을 다룬다.

- Parallel Build
- Worker Pool
- Task Scheduler
- Build Events
- Resource Locking
- Concurrency
- Thread Safety
- Build Metrics

---

# Part 4. Parallel Build & Scheduler

---

# 66. Overview

Builder는 Multi-Core 환경을 최대한 활용하기 위해 Parallel Build를 지원한다.

그러나 Build 순서는 반드시 Dependency Graph를 준수해야 한다.

즉,

> **병렬 실행은 허용하지만 Dependency를 위반하는 실행은 절대 허용하지 않는다.**

---

# 67. Parallel Build Model

Builder는 DAG(Directed Acyclic Graph)를 기반으로 병렬 실행한다.

```
             Manifest
                 │
      ┌──────────┴──────────┐
      ▼                     ▼
    Type A              Type B
      │                     │
      ▼                     ▼
 Expression A         Expression B
      │                     │
      └──────────┬──────────┘
                 ▼
             Registry
```

동시에 실행 가능한 Node만 Worker에게 할당한다.

---

# 68. Scheduler Architecture

```
                 Build Queue

                      │

              Ready Node Queue

                      │

        ┌─────────────┼─────────────┐

        ▼             ▼             ▼

    Worker 1      Worker 2      Worker 3

        │             │             │

        └─────────────┼─────────────┘

                      ▼

              Build Context Update
```

Scheduler는 Worker에게 Ready Node를 분배한다.

---

# 69. Ready Queue

Ready Queue에는 Dependency가 모두 해결된 Node만 존재한다.

예시

```
Ready

Type A

Type B

Type C
```

Worker는 Ready Queue에서 Node를 가져온다.

---

# 70. Worker Pool

Worker Pool은 Build Task를 실행한다.

```text
WorkerPool

workers[]

submit()

shutdown()

await()
```

Worker 수는 Configuration에서 결정한다.

---

# 71. Worker Lifecycle

```
Start

↓

Wait Task

↓

Execute

↓

Update Graph

↓

Request Next Task

↓

Repeat

↓

Shutdown
```

Worker는 상태를 가지지 않는다.

---

# 72. Dynamic Scheduling

Worker가 작업을 완료하면

새로운 Ready Node를 즉시 가져온다.

```
Worker Finish

↓

Dependency Update

↓

Ready Queue

↓

Worker Fetch
```

Idle Worker가 발생하지 않도록 한다.

---

# 73. Task Granularity

Task는 Resource 단위이다.

예시

```
Grammar

Expression

Locale

Metadata
```

Plugin 전체를 하나의 Task로 처리하지 않는다.

---

# 74. Build Barrier

일부 작업은 병렬 수행이 불가능하다.

예시

```
Generate Registry

↓

Package Build

↓

Manifest Write
```

Barrier에서는 모든 Worker가 종료될 때까지 대기한다.

---

# 75. Thread Safety

Builder의 공유 데이터는 반드시 Thread Safe해야 한다.

공유 대상

- Registry
- Build Context
- Cache
- Diagnostics
- Metrics

동시 수정은 Lock 또는 Atomic 연산으로 보호한다.

---

# 76. Resource Locking

동일 Resource는 동시에 Build할 수 없다.

```
Worker A

↓

Grammar A
```

```
Worker B

↓

Grammar A

×

Denied
```

Node 단위 Lock을 사용한다.

---

# 77. Dependency Synchronization

Node 완료 시

Dependency Counter를 감소시킨다.

```
Type Finished

↓

Expression Dependency Count--

↓

Zero?

↓

Ready Queue
```

Dependency Counter는 Atomic하게 관리한다.

---

# 78. Build Context Synchronization

Build Context는 병렬로 갱신된다.

예시

```
Worker

↓

Registry Entry

↓

Context Merge
```

Merge는 순서와 무관하게 동일한 결과를 생성해야 한다.

---

# 79. Event System

Builder는 Build Event를 발생시킨다.

예시

```text
BuildStarted

ResourceDiscovered

NodeReady

NodeBuilt

RegistryGenerated

BuildFinished
```

이벤트는 Logging 및 Plugin Hook에서 사용된다.

---

# 80. Plugin Hooks

Builder는 Hook을 지원한다.

```text
beforeBuild()

afterBuild()

beforeRegistry()

afterRegistry()

buildCompleted()
```

Hook은 Build 결과를 변경해서는 안 된다.

---

# 81. Cancellation

Builder는 Build 취소를 지원한다.

```
Cancel Request

↓

Stop Scheduling

↓

Finish Running Tasks

↓

Save State

↓

Shutdown
```

강제 종료보다 안전한 종료를 우선한다.

---

# 82. Failure Handling

Worker 실패 시

```
Worker Error

↓

Node Failed

↓

Cancel Dependents

↓

Continue Remaining

↓

Summary
```

Fatal Error가 아닌 경우 가능한 범위까지 Build를 계속 수행한다.

---

# 83. Build Metrics

Builder는 다음 정보를 수집한다.

- Build Time
- Queue Wait Time
- Worker Utilization
- Cache Hit Ratio
- Dirty Resource Count
- Registry Size

Metrics는 성능 분석에 활용된다.

---

# 84. Configuration

예시

```yaml
parallel: true

workers: auto

maxWorkers: 16

scheduler: dynamic

resourceLock: true

metrics: true
```

auto는 CPU Core 수를 기준으로 Worker를 결정한다.

---

# 85. Performance Targets

권장 목표

| Metric | Target |
|----------|---------|
| Worker Utilization | >90% |
| Scheduler Overhead | <2% |
| Queue Wait | <5ms |
| Registry Merge | O(1) 평균 |
| Lock Contention | 최소화 |

---

# 86. Public Interfaces

```text
IScheduler

schedule()

enqueue()

dequeue()

cancel()
```

---

```text
IWorkerPool

submit()

await()

shutdown()
```

---

```text
IBuildEventBus

publish()

subscribe()

unsubscribe()
```

---

# 87. Example Execution

```
Ready Queue

↓

Worker A → Grammar A

Worker B → Grammar B

Worker C → Grammar C

↓

Complete

↓

Registry Barrier

↓

Registry Build

↓

Done
```

---

# 88. Pseudocode

```text
while readyQueue.notEmpty():

    worker = acquireWorker()

    node = readyQueue.pop()

    worker.execute(node)

    updateDependencyCounters()

    enqueueNewReadyNodes()
```

---

# 89. Testing Requirements

Parallel Build는 다음 항목을 검증해야 한다.

- Race Condition
- Deadlock
- Resource Lock
- Dependency Ordering
- Worker Cancellation
- Barrier Synchronization
- Metrics Collection
- Hook Execution
- Large Repository Stress Test

---

# 90. Part 4 Summary

Builder는 Dependency Graph를 기반으로 병렬 실행을 수행하며, Scheduler와 Worker Pool을 통해 Ready Node를 효율적으로 처리한다.

모든 병렬 처리는 Build 결과의 결정성(Determinism)을 유지해야 하며, Registry 생성과 같은 전역 작업은 Build Barrier를 통해 순서를 보장한다.

Part 5에서는 다음 내용을 다룬다.

- Diagnostics
- Error Reporting
- Logging Strategy
- Testing Strategy
- Extension Model
- Public API
- Best Practices
- Future Improvements

---

# Part 5. Diagnostics, Testing & Extension

---

# 91. Overview

Builder는 단순히 Resource를 Build하는 도구가 아니다.

Builder는 Repository의 상태를 분석하고, 문제를 보고하며, 다른 컴포넌트가 활용할 수 있는 진단 정보를 생성하는 역할도 수행한다.

Builder의 출력은 항상 다음 세 가지를 포함해야 한다.

- Build Context
- Build Report
- Diagnostics

---

# 92. Diagnostic Model

Builder는 모든 문제를 Diagnostic으로 표현한다.

```text
Diagnostic

id

severity

message

resource

location

category

code

suggestion
```

Diagnostic은 사람이 읽을 수 있어야 하며 기계도 처리할 수 있어야 한다.

---

# 93. Severity Levels

Builder는 다음 Severity를 사용한다.

| Level | Description |
|---------|-------------|
| INFO | 참고 정보 |
| WARNING | Build 가능하지만 수정 권장 |
| ERROR | Resource Build 실패 |
| FATAL | Build 즉시 중단 |

Severity는 RFC-0008 Validator Specification과 동일한 체계를 따른다.

---

# 94. Diagnostic Categories

Builder는 문제를 다음과 같이 분류한다.

- Discovery
- Parsing
- Dependency
- Registry
- Manifest
- Plugin
- Configuration
- Cache
- Internal

예시

```
Dependency

↓

Missing Resource
```

---

# 95. Error Reporting

Builder는 오류를 누락하지 않는다.

예시

```
ERROR

Plugin

SkBee

Resource

expression/foo.json

Reason

Missing Type

player
```

가능하면 원인과 해결 방법을 함께 제공한다.

---

# 96. Build Report

Build 완료 후 Build Report를 생성한다.

예시

```text
Build Report

Plugins: 8

Resources: 4,932

Dirty Resources: 18

Cache Hit: 97.4%

Warnings: 2

Errors: 0

Elapsed: 1.84 sec
```

Report는 CI Artifact로 저장할 수 있다.

---

# 97. Structured Logging

Builder는 구조화된 로그를 사용한다.

예시(JSON)

```json
{
  "level": "INFO",
  "component": "Builder",
  "plugin": "Skript",
  "resource": "effects/spawn.json",
  "message": "Resource parsed successfully"
}
```

Plain Text 로그는 선택 사항이다.

---

# 98. Debug Mode

Debug Mode에서는 추가 정보를 출력한다.

포함 항목

- Dependency Resolution
- Cache Lookup
- Dirty Detection
- Build Queue
- Registry Merge
- Scheduler State

Debug 출력은 일반 Build에서는 비활성화한다.

---

# 99. Testing Strategy

Builder는 다음 수준의 테스트를 가진다.

### Unit Test

- Resource Loader
- Parser
- Graph
- Scheduler
- Cache

---

### Integration Test

- Full Build
- Incremental Build
- Registry Generation
- Plugin Build

---

### End-to-End Test

```
Repository

↓

Builder

↓

Validator

↓

Generator
```

전체 Build Pipeline을 검증한다.

---

# 100. Test Fixtures

모든 테스트는 Fixture Repository를 사용한다.

예시

```text
fixtures/

simple/

multi-plugin/

circular/

invalid-schema/

large/

incremental/
```

Fixture는 재현 가능한 테스트 환경을 제공한다.

---

# 101. Performance Testing

Builder는 성능 테스트를 포함해야 한다.

측정 항목

- Build Time
- Memory Usage
- Cache Hit Ratio
- Scheduler Efficiency
- Registry Generation Time

Regression Test를 통해 성능 저하를 감지한다.

---

# 102. Benchmarking

대표적인 Benchmark 시나리오

| Scenario | Resources |
|-----------|----------:|
| Small | 500 |
| Medium | 5,000 |
| Large | 50,000 |
| Enterprise | 200,000+ |

모든 Benchmark는 동일한 Fixture를 사용한다.

---

# 103. Public API

Builder는 명확한 Public API를 제공한다.

```text
IBuilder

initialize()

build()

incrementalBuild()

clean()

shutdown()
```

Builder 내부 구현은 Public API를 통해 노출하지 않는다.

---

# 104. Build Hooks

Builder는 확장 가능한 Hook을 제공한다.

```text
beforeDiscovery()

afterDiscovery()

beforeParsing()

afterParsing()

beforeRegistry()

afterRegistry()

beforeFinish()

afterFinish()
```

Hook은 Build 결과를 변경하지 않는 것을 원칙으로 한다.

---

# 105. Plugin Extensions

Plugin은 Builder 기능을 확장할 수 있다.

지원 예시

- Custom Resource Loader
- Custom Parser
- Custom Discovery Rule
- Custom Metadata Provider

Builder Core는 Plugin 구현을 직접 알지 못한다.

---

# 106. Configuration Profiles

Builder는 Profile 기반 설정을 지원한다.

예시

```yaml
profile: development
```

```yaml
profile: ci
```

```yaml
profile: release
```

Profile은 Cache, Logging, Worker 수 등의 기본값을 정의한다.

---

# 107. Best Practices

Builder 구현 시 다음 원칙을 권장한다.

- 순수 함수(Pure Function)를 우선 사용한다.
- 전역 상태(Global State)를 최소화한다.
- Resource는 Immutable Model로 관리한다.
- Build Context만 공유한다.
- Registry를 단일 조회 지점으로 사용한다.
- 예외(Exception)보다 명시적인 오류(Result/Diagnostic)를 선호한다.

---

# 108. Future Improvements

향후 고려 가능한 기능

- Remote Build Cache
- Distributed Build
- Build Farm 지원
- File Watch Mode
- Hot Reload
- Build Visualization
- Incremental Registry Merge
- Build Trace Viewer

이 기능들은 현재 범위에는 포함되지 않는다.

---

# 109. Compliance Checklist

Builder 구현은 다음 항목을 만족해야 한다.

- RFC-0007 Builder Specification 준수
- ADR-0003 Incremental Build Architecture 준수
- ADR-0004 Registry as Single Source of Truth 준수
- ADR-0009 Unified Caching Strategy 준수
- SPEC-0010 Dependency Resolution 준수

CI에서는 Compliance Checklist를 자동 검증할 수 있다.

---

# 110. Builder Lifecycle Summary

전체 Builder Lifecycle

```
Initialize

↓

Load Configuration

↓

Discover Resources

↓

Load Files

↓

Parse Resources

↓

Resolve Dependencies

↓

Generate Build Graph

↓

Execute Build

↓

Generate Registry

↓

Create Build Context

↓

Generate Diagnostics

↓

Write Build Report

↓

Shutdown
```

Builder는 Repository를 Build Context로 변환하는 역할만 수행한다.

Validation, Packaging 및 DataPack 생성은 각각 Validator와 Generator의 책임이다.

---

# 111. Document Summary

본 문서는 Builder의 구현 기준을 정의한다.

Builder는 다음 원칙을 따른다.

- Deterministic Build
- Incremental Build
- Immutable Input
- Dependency-aware Scheduling
- Unified Caching
- Registry-based Resolution
- Structured Diagnostics
- Extensible Architecture

모든 구현은 본 문서와 RFC 및 ADR에서 정의한 계약(Contract)을 준수해야 한다.

---

# Revision History

| Version | Description |
|----------|-------------|
| 1.0 | Initial Implementation Guide |