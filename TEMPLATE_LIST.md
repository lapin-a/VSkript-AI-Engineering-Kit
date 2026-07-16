# TEMPLATE_LIST.md

# VSkript AI Engineering Kit

> Master Template & Document Index

Version: 1.0

---

# 1. Purpose

본 문서는 VSkript AI Engineering Kit에서 관리하는 모든 문서, Prompt, Template, Report 및 Checklist를 정의한다.

이 문서는 프로젝트의 "목차(Index)" 역할을 수행하며, 새로운 문서가 추가될 경우 반드시 본 문서를 먼저 수정해야 한다.

---

# 2. Document Categories

프로젝트는 다음 8개의 카테고리로 구성된다.

| Category | Purpose |
|----------|----------|
| Foundation | 프로젝트 표준 정의 |
| Analysis | 프로젝트 분석 |
| Development | 기능 구현 |
| Verification | 검증 |
| Audit | 코드 감사 |
| Reporting | 보고서 |
| Templates | 템플릿 |
| Examples | 예제 |

---

# 3. Foundation Documents

프로젝트의 최상위 문서이다.

| File | Description | Status |
|------|-------------|--------|
| README.md | 프로젝트 소개 | Required |
| PROJECT.md | 프로젝트 목표 | Required |
| REQUIREMENTS.md | 요구사항 | Required |
| WORKFLOW.md | 작업 흐름 | Required |
| RULES.md | 프로젝트 규칙 | Required |
| STYLE_GUIDE.md | 문서 작성 규칙 | Required |
| PROMPT_STANDARD.md | Prompt 표준 | Required |
| REPORT_STANDARD.md | Report 표준 | Required |
| DIRECTORY.md | 디렉터리 구조 | Required |
| TEMPLATE_LIST.md | 문서 목록 | Required |
| ROADMAP.md | 개발 계획 | Required |
| GLOSSARY.md | 용어 사전 | Recommended |
| QUALITY_GATE.md | 품질 기준 | Required |
| AI_GUIDELINES.md | AI 행동 규칙 | Required |
| CONTRIBUTING.md | 기여 가이드 | Required |
| CHANGELOG.md | 변경 이력 | Required |

---

# 4. Analysis Prompts

프로젝트 분석 단계에서 사용하는 Prompt

| File | Purpose |
|------|----------|
| 01_Project_Analysis.md | 프로젝트 전체 분석 |
| 02_Architecture_Review.md | 구조 검토 |
| 03_Dependency_Analysis.md | 의존성 분석 |
| 04_Performance_Analysis.md | 성능 분석 |

---

# 5. Development Prompts

기능 개발 단계에서 사용한다.

| File | Purpose |
|------|----------|
| 10_Implementation.md | 기능 구현 |
| 11_Refactoring.md | 리팩터링 |
| 12_Database_Architecture.md | Database 설계 |
| 13_DataPack_System.md | DataPack 시스템 |
| 14_Performance_Optimization.md | 성능 최적화 |

---

# 6. Verification Prompts

구현 이후 수행하는 검증 Prompt

| File | Purpose |
|------|----------|
| 20_Static_Verification.md | 정적 검증 |
| 21_Functional_Verification.md | 기능 검증 |
| 22_Runtime_Test.md | Runtime Test |
| 23_Regression_Test.md | 회귀 테스트 |
| 24_Code_Audit.md | 코드 감사 |
| 25_Security_Review.md | 보안 검토 |
| 26_Final_Report.md | 최종 보고서 |

---

# 7. Report Templates

| Template | Purpose |
|-----------|----------|
| Analysis_Report.md | 분석 보고서 |
| Verification_Report.md | 검증 보고서 |
| Audit_Report.md | 코드 감사 |
| Performance_Report.md | 성능 분석 |
| Final_Report.md | 최종 보고서 |

---

# 8. Prompt Templates

모든 Prompt 작성 시 사용하는 공통 템플릿

| Template | Purpose |
|-----------|----------|
| Analysis_Template.md | 분석 Prompt |
| Development_Template.md | 개발 Prompt |
| Verification_Template.md | 검증 Prompt |
| Audit_Template.md | 감사 Prompt |
| Documentation_Template.md | 문서 Prompt |

---

# 9. Checklists

모든 작업 완료 전에 확인한다.

| Checklist | Purpose |
|------------|----------|
| Analysis_Checklist.md | 분석 완료 |
| Development_Checklist.md | 구현 완료 |
| Verification_Checklist.md | 검증 완료 |
| Runtime_Checklist.md | 실행 테스트 |
| Release_Checklist.md | 릴리즈 |
| Audit_Checklist.md | 코드 감사 |

---

# 10. Workflow Documents

| Workflow | Purpose |
|-----------|----------|
| Analysis_Workflow.md | 분석 절차 |
| Development_Workflow.md | 개발 절차 |
| Verification_Workflow.md | 검증 절차 |
| Release_Workflow.md | 릴리즈 절차 |

---

# 11. Example Documents

실제 프로젝트 예제

| Example | Purpose |
|----------|----------|
| Example_Project_Analysis.md | 분석 예제 |
| Example_Verification.md | 검증 예제 |
| Example_Report.md | 보고서 예제 |
| Example_Code_Audit.md | 감사 예제 |

---

# 12. Future Documents

향후 추가 예정

## AI
- AI_Behavior.md
- AI_Limitations.md

## Documentation
- RELEASE_NOTE.md

---

## Automation

- Automation_Guide.md
- CI_Guide.md
- GitHub_Actions.md

---

## DataPack

- DataPack_Guide.md
- Manifest_Specification.md
- Registry_Guide.md

---

# 13. Document Lifecycle

모든 문서는 다음 상태를 가진다.

| Status | Description |
|---------|-------------|
| Draft | 작성 중 |
| Review | 검토 중 |
| Approved | 승인 |
| Deprecated | 폐기 |
| Archived | 보관 |

---

# 14. Naming Convention

Prompt

```
01_Project_Analysis.md
```

Template

```
Verification_Template.md
```

Checklist

```
Runtime_Checklist.md
```

Workflow

```
Development_Workflow.md
```

---

# 15. Maintenance Policy

새로운 Prompt를 추가하는 경우

반드시

- TEMPLATE_LIST.md
- DIRECTORY.md
- ROADMAP.md

를 함께 수정한다.

---

# 16. Quality Checklist

□ 모든 Prompt 등록

□ 모든 Template 등록

□ 모든 Workflow 등록

□ 모든 Checklist 등록

□ 최신 상태 유지

---

# 17. Document Information

| Item | Value |
|------|-------|
| Document | TEMPLATE_LIST.md |
| Version | 1.0 |
| Status | Draft |
| Owner | VSkript AI Engineering Kit |
| Last Updated | YYYY-MM-DD |

---

# Summary

TEMPLATE_LIST.md는 VSkript AI Engineering Kit의 모든 문서를 관리하는 Master Index이다.

새로운 문서를 생성하거나 삭제할 때 가장 먼저 수정해야 하는 기준 문서이며, 프로젝트의 전체 구조를 한눈에 파악할 수 있도록 유지한다.
