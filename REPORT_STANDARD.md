# REPORT_STANDARD.md

# VSkript AI Engineering Kit
## Report Standard

Version: 2.0

Status: Draft

---

# 1. Purpose

본 문서는 VSkript AI Engineering Kit에서 생성되는 모든 Report의 표준 구조를 정의한다.

Report는 프로젝트 분석, 구현, 검증, 감사 결과를 일관된 형식으로 기록하기 위한 공식 문서이다.

---

# 2. Goals

Report Standard의 목적은 다음과 같다.

- 보고서 형식 표준화
- Evidence 기반 결과 기록
- 재현 가능한 검증
- 프로젝트 간 일관성 확보
- AI 모델 독립성 유지

---

# 3. Report Philosophy

모든 Report는 다음 원칙을 따른다.

- Evidence First
- No Guessing
- Specification First
- Transparency
- Reproducibility
- Traceability

---

# 4. Report Categories

프로젝트에서는 다음 Report를 사용한다.

## Analysis Report

프로젝트 구조 분석

예)

- Project Analysis
- Architecture Analysis
- Dependency Analysis

---

## Development Report

구현 결과 보고

예)

- Feature Development
- Refactoring

---

## Verification Report

검증 결과

예)

- Static Verification
- Functional Verification
- Runtime Verification

---

## Audit Report

품질 감사

예)

- Code Audit
- Documentation Audit
- Specification Audit

---

## Final Report

최종 결과 보고

---

# 5. Standard Report Structure

모든 Report는 다음 구조를 따른다.

```text
Title

↓

Executive Summary

↓

Objectives

↓

Scope

↓

Methodology

↓

Findings

↓

Evidence

↓

Assessment

↓

Recommendations

↓

Conclusion
```

---

# 6. Required Sections

모든 Report는 다음 항목을 포함해야 한다.

| Section | Required |
|----------|----------|
| Executive Summary | YES |
| Objectives | YES |
| Scope | YES |
| Methodology | YES |
| Findings | YES |
| Evidence | YES |
| Assessment | YES |
| Conclusion | YES |

---

# 7. Executive Summary

Executive Summary에는 다음 내용을 포함한다.

- 작업 목적
- 수행 범위
- 주요 결과
- 최종 평가

---

# 8. Scope

Report의 범위를 명확히 정의한다.

예)

- Source Code
- Documentation
- Specification
- Runtime
- DataPack

---

# 9. Methodology

수행 절차를 기록한다.

예)

- Static Analysis
- Runtime Verification
- Manual Review
- Specification Comparison

---

# 10. Findings

발견 사항을 기록한다.

각 Finding은 다음 구조를 따른다.

```text
Finding

↓

Description

↓

Evidence

↓

Impact

↓

Recommendation
```

---

# 11. Evidence

모든 Finding은 Evidence를 가져야 한다.

Evidence 예시

- Source Code
- Line Number
- Configuration
- Runtime Log
- Specification
- Screenshot

Evidence가 없는 내용은 사실로 단정하지 않는다.

---

# 12. Assessment

결과는 다음 등급으로 분류한다.

- PASS
- VERIFIED
- PARTIALLY VERIFIED
- WARNING
- FAIL
- FALSE POSITIVE
- NEEDS RUNTIME TEST
- NOT APPLICABLE

---

# 13. Recommendations

Recommendation은 다음 원칙을 따른다.

- 사실 기반
- 구현 강요 금지
- 개선 방향 제시
- 우선순위 명시

---

# 14. Severity

Finding은 다음 Severity를 가진다.

Critical

High

Medium

Low

Informational

---

# 15. Traceability

모든 Finding은 다음과 연결되어야 한다.

Requirement

↓

Specification

↓

Implementation

↓

Evidence

↓

Finding

---

# 16. Quality Requirements

Report는 다음 조건을 만족해야 한다.

□ Evidence 존재

□ 추측 없음

□ 구조 일관성

□ 용어 일관성

□ 재현 가능

□ 출처 명확

---

# 17. Report Naming

파일명 규칙

```
analysis_report.md

verification_report.md

audit_report.md

runtime_report.md

final_report.md
```

---

# 18. Report Directory

```text
reports/

analysis/

verification/

audit/

runtime/

release/
```

---

# 19. Review

Report는 다음 절차를 따른다.

```text
Draft

↓

Review

↓

Verification

↓

Approved

↓

Published
```

---

# 20. Definition of Done

Report는 다음 조건을 만족해야 완료된다.

- Required Sections 포함
- Evidence 첨부
- Assessment 완료
- Conclusion 작성
- Review 완료

---

# 21. Summary

Report Standard는 VSkript AI Engineering Kit에서 생성되는 모든 보고서의 공식 형식을 정의한다.

모든 Report는 동일한 구조를 사용하며, Evidence 기반의 재현 가능한 결과를 기록하는 것을 목표로 한다.