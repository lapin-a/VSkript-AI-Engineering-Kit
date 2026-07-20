# RULES.md 

# VSkript AI Engineering Kit

> Global Rules and Engineering Standards

Version: 1.0

---

# 1. Purpose

본 문서는 VSkript AI Engineering Kit에서 사용하는 모든 AI가 반드시 따라야 하는 공통 규칙을 정의한다.

모든 Prompt, Workflow, Template, Report 및 문서는 본 규칙을 우선적으로 따른다.

본 문서보다 하위 문서가 우선할 수 없으며, 충돌이 발생하는 경우 RULES.md를 기준으로 판단한다.

---

# 2. Core Principles

모든 작업은 아래 원칙을 따른다.

1. Evidence First
2. No Guessing
3. Project First
4. Safety First
5. Maintainability First
6. Transparency
7. Consistency

---

# 3. Mandatory Rules

## Rule 1 — Analyze Before Modify

절대 프로젝트를 분석하지 않고 구현하지 않는다.

반드시

- 프로젝트 구조
- 실행 흐름
- 데이터 흐름

을 이해한 후 작업한다.

---

## Rule 2 — Evidence-Based Decisions

모든 결론은 실제 프로젝트 구조 또는 코드에 근거해야 한다.

허용

- 코드
- 설정 파일
- 문서
- 실행 흐름

허용되지 않음

- 추측
- 가정
- 경험 기반 추론

---

## Rule 3 — Required File Exploration

필요한 파일은 AI가 직접 찾아야 한다.

예시

- package.json
- tsconfig.json
- extension.ts
- src/
- providers/
- database/
- build/

필요하다면 같은 파일을 여러 번 열람할 수 있다.

---

## Rule 4 — Never Stop Due to Missing Exploration

다음과 같은 이유로 분석을 중단해서는 안 된다.

- 파일을 읽지 않았다.
- 파일이 너무 많다.
- 필요한 파일을 찾지 못했다.

필요한 파일을 스스로 탐색한 후 계속 진행한다.

---

## Rule 5 — Preserve Existing Behavior

새로운 기능을 구현할 때 기존 기능을 최대한 유지해야 한다.

다음 기능은 특별한 이유가 없는 한 변경하지 않는다.

- Completion
- Hover
- Signature Help
- Definition
- References

---

## Rule 6 — Minimize Changes

필요 이상의 리팩터링을 수행하지 않는다.

영향 범위를 최소화한다.

---

## Rule 7 — Explain Every Change

모든 변경 사항은 이유를 설명해야 한다.

포함

- 변경 목적
- 기대 효과
- 영향 범위

---

## Rule 8 — Runtime Separation

실행해야만 확인 가능한 항목은 반드시 분리한다.

예시

- VSCode 실행
- Hover 확인
- Completion 확인

---

## Rule 9 — Verification Required

구현 후 반드시 검증을 수행한다.

---

## Rule 10 — Documentation Required

모든 중요한 변경에는 문서를 함께 작성한다.

---

# 4. Analysis Rules

프로젝트 분석 시 반드시 확인한다.

- Directory Structure
- Architecture
- Providers
- Database
- Build
- Configuration
- Registry
- Cache
- Manifest
- DataPack

---

# 5. Development Rules

구현 시 다음을 우선 고려한다.

## Maintainability

유지보수가 쉬운가

---

## Readability

가독성이 좋은가

---

## Extensibility

새로운 기능을 쉽게 추가할 수 있는가

---

## Performance

성능이 유지되는가

---

## Compatibility

기존 기능과 호환되는가

---

# 6. Verification Rules

모든 검증은 다음 상태를 사용한다.

| Status | Meaning |
|---------|----------|
| VERIFIED | 코드와 요구사항이 일치 |
| PARTIALLY VERIFIED | 일부 확인 |
| NEEDS RUNTIME TEST | 실행 환경 필요 |
| ISSUE | 문제 발견 |
| NOT APPLICABLE | 해당 없음 |

---

## 모든 판정에는 반드시 포함

- Evidence
- Reason
- Impact
- Recommendation

---

# 7. Reporting Rules

모든 보고서는 아래 구조를 따른다.

1. Executive Summary
2. Scope
3. Inputs
4. Methodology
5. Findings
6. Evidence
7. Impact Analysis
8. Risk Assessment
9. Recommendations
10. Verification Result
11. Next Steps
12. Appendix

---

# 8. Coding Standards

권장

- SOLID
- DRY
- KISS
- SRP
- OCP

지양

- Magic Number
- Hard Coding
- Duplicate Logic
- Circular Dependency

---

# 9. Prompt Standards

모든 Prompt는 다음 구조를 따른다. 상세 정의는 PROMPT_STANDARD.md를 기준으로 한다.

- Purpose
- Scope
- Role
- Required Skills
- Context
- Inputs
- Expected Outputs
- Constraints
- Rules
- Workflow
- Evidence Policy
- Verification Policy
- Quality Checklist
- Completion Criteria

---

# 10. Quality Checklist

AI는 항상 다음을 확인한다.

□ 프로젝트 전체를 분석했는가

□ 필요한 파일을 직접 탐색했는가

□ 추측하지 않았는가

□ 코드 근거를 제시했는가

□ 영향 범위를 분석했는가

□ Runtime Test를 분리했는가

□ 보고서를 작성했는가

---

# 11. Common Mistakes

금지

- 일부 파일만 보고 결론

- 코드 근거 없는 판단

- Runtime Test 생략

- 영향 범위 미분석

- 구조를 이해하지 않은 구현

---

# 12. AI Behavior Policy

AI는 다음 행동을 수행한다.

항상

- 구조를 먼저 이해한다.
- 필요한 파일을 탐색한다.
- 증거를 수집한다.
- 분석 후 구현한다.
- 구현 후 검증한다.
- 검증 후 보고서를 작성한다.

---

# 13. Document Priority

우선순위

1. RULES.md
2. PROJECT.md
3. REQUIREMENTS.md
4. WORKFLOW.md
5. AI_GUIDELINES.md / PROMPT_STANDARD.md / REPORT_STANDARD.md / STYLE_GUIDE.md / QUALITY_GATE.md / DIRECTORY.md / TEMPLATE_LIST.md / GLOSSARY.md
6. Prompt Files
7. Templates
8. Reports

---

# 14. Definition of Success

작업은 다음 조건을 모두 만족해야 성공으로 간주한다.

- 요구사항 충족
- 프로젝트 규칙 준수
- 코드 근거 제시
- 검증 완료
- Runtime Test 구분
- 보고서 작성 완료

---

# 15. Document Information

| Item | Value |
|------|-------|
| Document | RULES.md |
| Version | 1.0 |
| Status | Draft |
| Owner | VSkript AI Engineering Kit |
| Last Updated | YYYY-MM-DD |

---

# Summary

RULES.md는 VSkript AI Engineering Kit의 최상위 규칙 문서이다.

모든 프롬프트와 문서는 본 규칙을 기반으로 작성되며, AI는 프로젝트를 분석하고 구현하고 검증하는 모든 과정에서 본 문서를 최우선 기준으로 사용해야 한다.
