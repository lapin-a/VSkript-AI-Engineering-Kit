# QUALITY_GATE.md

# VSkript AI Engineering Kit

> Quality Gates & Definition of Done

Version: 1.0

---

# 1. Purpose

본 문서는 VSkript AI Engineering Kit의 모든 작업이 충족해야 하는 품질 기준(Quality Gate)을 정의한다.

모든 프로젝트 분석, 기능 구현, 리팩터링, 검증 및 문서화는 본 문서의 기준을 통과해야 완료로 간주한다.

본 문서는 "Definition of Done(DoD)"의 기준 문서이다.

---

# 2. Objectives

Quality Gate의 목표는 다음과 같다.

- AI 결과물의 품질 표준화
- 재현 가능한 작업 보장
- 근거(Evidence) 기반 결과 생성
- 검증 가능한 결과 제공
- 장기 유지보수성 확보

---

# 3. Quality Gate Overview

모든 작업은 아래 순서를 통과해야 한다.

```text
Project Understanding
        │
        ▼
Architecture Validation
        │
        ▼
Implementation Validation
        │
        ▼
Verification
        │
        ▼
Documentation
        │
        ▼
Final Review
        │
        ▼
Completed
```

---

# 4. Gate 1 — Project Understanding

## Goal

프로젝트를 충분히 이해했는가?

### Required

- 프로젝트 구조 분석
- 주요 모듈 식별
- 데이터 흐름 분석
- 실행 흐름 분석
- 핵심 Provider 확인

### Pass Criteria

- 프로젝트 요약 작성 완료
- 주요 구조 설명 가능
- 변경 대상 식별 완료

---

# 5. Gate 2 — Architecture Validation

## Goal

변경이 기존 구조와 충돌하지 않는가?

### Required

- Layer 확인
- Module 관계 확인
- Dependency 분석
- SOLID 검토
- 확장성 검토

### Pass Criteria

- 구조 유지
- 책임 분리 유지
- 불필요한 결합 없음

---

# 6. Gate 3 — Implementation Validation

## Goal

구현이 요구사항을 충족하는가?

### Required

- 요구사항 반영
- 변경 이유 설명
- 영향 범위 분석
- 기존 기능 유지

### Pass Criteria

- 요구사항 충족
- 영향도 문서화
- 기존 기능 손상 없음

---

# 7. Gate 4 — Verification

## Goal

구현 결과를 검증했는가?

### Static Verification

- 수정 파일 비교
- 신규 파일 확인
- 삭제 파일 확인
- 영향 분석

### Functional Verification

- Completion
- Hover
- Signature Help
- Definition
- References
- Diagnostics

### Runtime Verification

- VSCode 실행
- Extension 활성화
- Plugin Detection
- DataPack Loading

### Pass Criteria

- 정적 검증 완료
- Runtime Test 필요 여부 명시
- 결과 상태 분류 완료

---

# 8. Gate 5 — Documentation

## Goal

필요한 문서가 모두 작성되었는가?

### Required

- 변경 이유
- 구조 설명
- 검증 결과
- Runtime Checklist
- 최종 보고서

### Pass Criteria

- Markdown 표준 준수
- 문서 최신화
- 버전 정보 포함

---

# 9. Gate 6 — Final Review

## Goal

전체 작업을 최종 검토했는가?

### Required

- Evidence 확인
- Risk 확인
- Recommendation 작성
- Next Step 작성

### Pass Criteria

- 모든 Quality Gate 통과
- 최종 보고서 완료

---

# 10. Evidence Requirements

모든 작업은 아래 근거 중 하나 이상을 포함해야 한다.

허용

- Source Code
- Configuration
- Build Files
- Runtime Result
- Test Result
- Documentation

허용되지 않음

- 추측
- 일반적인 경험
- 확인되지 않은 정보

---

# 11. Verification Status

모든 결과는 아래 상태를 사용한다.

| Status | Description |
|---------|-------------|
| VERIFIED | 코드 또는 테스트로 확인됨 |
| PARTIALLY VERIFIED | 일부 확인됨 |
| NEEDS RUNTIME TEST | 실행 환경에서 확인 필요 |
| ISSUE | 문제 발견 |
| NOT APPLICABLE | 해당 없음 |

---

# 12. Risk Levels

| Level | Meaning |
|---------|----------|
| Critical | 즉시 수정 필요 |
| High | 높은 위험 |
| Medium | 개선 권장 |
| Low | 영향 적음 |
| None | 문제 없음 |

---

# 13. Mandatory Checklist

## Project Analysis

□ 프로젝트 구조 분석 완료

□ 주요 컴포넌트 확인

□ 데이터 흐름 분석

□ 실행 흐름 분석

---

## Implementation

□ 요구사항 충족

□ 영향도 분석

□ 변경 이유 기록

□ 기존 기능 유지

---

## Verification

□ Static Verification 완료

□ Functional Verification 완료

□ Runtime Test 분리

□ 결과 상태 기록

---

## Documentation

□ README 반영

□ 관련 문서 수정

□ 최종 보고서 작성

---

# 14. Definition of Done

작업은 아래 조건을 모두 만족해야 완료된다.

- 모든 Quality Gate 통과
- Evidence 제공
- Verification 완료
- Documentation 완료
- Runtime Test 필요 여부 명시
- Recommendation 작성

---

# 15. Failure Conditions

다음 중 하나라도 해당되면 완료로 인정하지 않는다.

- 프로젝트 분석 없이 구현
- Evidence 없음
- Verification 없음
- Runtime Test 누락
- 영향도 분석 없음
- 문서 미작성
- 요구사항 미충족

---

# 16. Continuous Improvement

Quality Gate는 프로젝트 발전에 따라 지속적으로 개선한다.

변경 시 반드시 다음 문서를 함께 검토한다.

- REQUIREMENTS.md
- WORKFLOW.md
- RULES.md
- REPORT_STANDARD.md

---

# 17. Metrics

품질 측정을 위해 다음 지표를 사용한다.

| Metric | Target |
|---------|--------|
| Evidence Coverage | 100% |
| Verification Coverage | 100% |
| Documentation Coverage | 100% |
| Prompt Compliance | 100% |
| Rule Compliance | 100% |

---

# 18. Exit Criteria

모든 Gate를 통과하고 아래 조건을 만족하면 작업을 종료한다.

- 품질 기준 충족
- 보고서 제출
- 문서 업데이트
- 후속 작업 제안

---

# 19. Document Information

| Item | Value |
|------|-------|
| Document | QUALITY_GATE.md |
| Version | 1.0 |
| Status | Draft |
| Owner | VSkript AI Engineering Kit |
| Last Updated | YYYY-MM-DD |

---

# Summary

QUALITY_GATE.md는 VSkript AI Engineering Kit의 모든 작업에 적용되는 품질 기준과 완료 조건(Definition of Done)을 정의한다.

모든 프로젝트 분석, 개발, 검증 및 문서화는 본 문서의 Quality Gate를 통과해야 완료로 인정되며, 이를 통해 AI 결과물의 신뢰성, 일관성 및 유지보수성을 보장한다.
