# REPORT_STANDARD.md 

# VSkript AI Engineering Kit

> Standard Report Specification

Version: 1.0

---

# 1. Purpose

본 문서는 VSkript AI Engineering Kit에서 생성되는 모든 보고서의 표준 형식을 정의한다.

모든 Analysis, Verification, Audit, Review 및 Summary Report는 본 문서를 기준으로 작성되어야 한다.

목표는 다음과 같다.

- 보고서 형식 통일
- 결과 비교 용이성 확보
- Evidence 기반 보고
- 추적 가능한 변경 이력 제공
- AI 간 동일한 출력 형식 유지

---

# 2. Report Types

본 표준은 다음 보고서에 적용된다.

| Report | Description |
|---------|-------------|
| Project Analysis | 프로젝트 분석 |
| Architecture Review | 구조 검토 |
| Dependency Analysis | 의존성 분석 |
| Performance Analysis | 성능 분석 |
| Static Verification | 정적 검증 |
| Functional Verification | 기능 검증 |
| Runtime Test | 실행 테스트 |
| Regression Test | 회귀 테스트 |
| Code Audit | 코드 감사 |
| Security Review | 보안 검토 |
| Final Report | 최종 보고서 |

---

# 3. Standard Report Structure

모든 보고서는 아래 순서를 따른다.

```

Executive Summary

Scope

Inputs

Methodology

Findings

Evidence

Impact Analysis

Risk Assessment

Recommendations

Verification Result

Next Steps

Appendix

```

---

# 4. Executive Summary

보고서의 핵심 내용을 5~10줄 이내로 요약한다.

포함 항목

- 목적
- 대상
- 주요 결과
- 결론

---

# 5. Scope

분석 범위를 정의한다.

예시

- Entire Repository
- Database Module
- Completion Provider
- Build System

---

# 6. Inputs

분석에 사용한 자료를 명시한다.

예시

- Source Code
- Before ZIP
- After ZIP
- README
- Configuration
- package.json

---

# 7. Methodology

어떤 방식으로 분석했는지 설명한다.

예시

- Static Analysis
- Code Comparison
- Dependency Inspection
- Configuration Review

---

# 8. Findings

발견한 내용을 기술한다.

각 항목은 다음 형식을 따른다.

## Finding

설명

### Evidence

코드 또는 구조

### Impact

영향

### Recommendation

개선안

---

# 9. Evidence

모든 결론은 Evidence를 포함해야 한다.

허용

- Source Code
- Directory Structure
- Build Output
- Runtime Result
- Configuration

금지

- 추측
- 일반적인 경험
- 근거 없는 판단

---

# 10. Impact Analysis

변경 사항이 영향을 미치는 범위를 설명한다.

분석 대상

- API
- Provider
- Registry
- Cache
- Build
- Database
- DataPack

---

# 11. Risk Assessment

위험도를 평가한다.

| Level | Meaning |
|---------|---------|
| Critical | 즉시 수정 필요 |
| High | 높은 위험 |
| Medium | 개선 권장 |
| Low | 영향 적음 |
| None | 문제 없음 |

---

# 12. Recommendations

개선 사항을 우선순위별로 작성한다.

Priority

- High
- Medium
- Low

각 항목은

- 문제
- 이유
- 개선 방향
- 예상 효과

를 포함한다.

---

# 13. Verification Result

모든 검증은 아래 상태를 사용한다.

| Status | Description |
|---------|-------------|
| VERIFIED | 코드 확인 완료 |
| PARTIALLY VERIFIED | 일부 확인 |
| NEEDS RUNTIME TEST | 실행 환경 필요 |
| ISSUE | 문제 발견 |
| NOT APPLICABLE | 해당 없음 |

---

# 14. Next Steps

다음 작업을 제안한다.

예시

- Runtime Test 수행
- 리팩터링
- 성능 개선
- 문서 업데이트

---

# 15. Appendix

필요 시 아래 내용을 포함한다.

- Directory Tree
- Tables
- File List
- Metrics
- Reference Links

---

# 16. Report Formatting Rules

제목은 H1부터 시작한다.

표를 적극 활용한다.

긴 설명은 Bullet List를 사용한다.

Code Block에는 언어를 지정한다.

Markdown 표준을 준수한다.

---

# 17. Quality Checklist

보고서 제출 전 확인한다.

□ Executive Summary 작성

□ Scope 명시

□ Inputs 명시

□ Methodology 설명

□ Evidence 포함

□ Impact 분석

□ Risk 평가

□ Recommendation 작성

□ Verification Status 포함

□ Next Steps 작성

---

# 18. Common Mistakes

다음 사항을 금지한다.

- Evidence 없는 결론

- Runtime Test와 정적 검증 혼용

- Recommendation 없는 보고서

- Risk 미평가

- Findings 없이 Summary만 작성

---

# 19. Report Template

```markdown
# Report Name

## Executive Summary

...

## Scope

...

## Inputs

...

## Methodology

...

## Findings

...

## Evidence

...

## Impact Analysis

...

## Risk Assessment

...

## Recommendations

...

## Verification Result

...

## Next Steps

...
```

---

# 20. Document Information

| Item | Value |
|------|-------|
| Document | REPORT_STANDARD.md |
| Version | 1.0 |
| Status | Draft |
| Owner | VSkript AI Engineering Kit |
| Last Updated | YYYY-MM-DD |

---

# Summary

REPORT_STANDARD.md는 VSkript AI Engineering Kit에서 생성되는 모든 분석 및 검증 보고서의 표준 형식을 정의한다.

모든 보고서는 본 문서의 구조를 따르며, Evidence 기반의 일관된 품질을 유지해야 한다.
