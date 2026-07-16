# DIRECTORY.md

# VSkript AI Engineering Kit

> Standard Directory Structure

Version: 1.0

---

# 1. Purpose

본 문서는 VSkript AI Engineering Kit의 표준 디렉터리 구조를 정의한다.

모든 문서, 프롬프트, 템플릿, 예제 및 체크리스트는 본 구조를 기준으로 관리한다.

목표는 다음과 같다.

- 프로젝트 구조 표준화
- 유지보수성 향상
- AI 자동 생성 지원
- 확장 가능한 구조 제공

---

# 2. Design Principles

디렉터리는 다음 원칙을 따른다.

- 기능별 분리
- 문서와 Prompt 분리
- 예제와 템플릿 분리
- 규칙과 체크리스트 분리
- 버전 관리 용이성 확보

---

# 3. Root Structure

```text
VSkript-AI-Engineering-Kit/

├── README.md
├── PROJECT.md
├── REQUIREMENTS.md
├── WORKFLOW.md
├── RULES.md
├── STYLE_GUIDE.md
├── PROMPT_STANDARD.md
├── REPORT_STANDARD.md
├── DIRECTORY.md
├── TEMPLATE_LIST.md
├── ROADMAP.md
├── LICENSE
├── CHANGELOG.md
├── CONTRIBUTING.md
│
├── docs/
├── prompts/
├── templates/
├── workflows/
├── reports/
├── checklists/
├── examples/
├── assets/
└── scripts/
```

---

# 4. Documentation Directory

```text
docs/

├── analysis/
├── architecture/
├── development/
├── verification/
├── audit/
├── database/
├── performance/
└── reference/
```

설명

| Directory | Purpose |
|------------|----------|
| analysis | 프로젝트 분석 문서 |
| architecture | 구조 설계 |
| development | 개발 가이드 |
| verification | 검증 문서 |
| audit | 코드 감사 |
| database | Database 설계 |
| performance | 성능 문서 |
| reference | 참고 자료 |

---

# 5. Prompt Directory

```text
prompts/

├── analysis/
├── development/
├── verification/
├── reporting/
├── templates/
└── shared/
```

---

## Analysis Prompts

```text
01_Project_Analysis.md

02_Architecture_Review.md

03_Dependency_Analysis.md

04_Performance_Analysis.md
```

---

## Development Prompts

```text
10_Implementation.md

11_Refactoring.md

12_Database_Architecture.md

13_DataPack_System.md

14_Performance_Optimization.md
```

---

## Verification Prompts

```text
20_Static_Verification.md

21_Functional_Verification.md

22_Runtime_Test.md

23_Regression_Test.md

24_Code_Audit.md

25_Security_Review.md

26_Final_Report.md
```

---

# 6. Templates Directory

```text
templates/

├── reports/
├── prompts/
├── issues/
├── pull_requests/
└── releases/
```

---

# 7. Reports Directory

```text
reports/

├── analysis/
├── verification/
├── audit/
├── performance/
└── release/
```

---

# 8. Checklists Directory

```text
checklists/

├── analysis.md
├── implementation.md
├── verification.md
├── runtime.md
├── release.md
└── audit.md
```

---

# 9. Examples Directory

```text
examples/

├── project-analysis/
├── verification/
├── reports/
├── prompts/
└── workflows/
```

---

# 10. Assets Directory

```text
assets/

├── diagrams/
├── images/
├── icons/
└── logos/
```

---

# 11. Scripts Directory

```text
scripts/

├── build/
├── validation/
├── formatting/
└── release/
```

---

# 12. Naming Convention

## Directories

모두 소문자를 사용한다.

예

```text
analysis
verification
templates
```

---

## Markdown Files

Pascal Case를 사용한다.

예

```text
Project_Analysis.md
```

Prompt 번호는 두 자리 숫자를 사용한다.

예

```text
01_Project_Analysis.md

21_Functional_Verification.md
```

---

# 13. Directory Rules

모든 Prompt는 prompts 디렉터리에 저장한다.

모든 예제는 examples에 저장한다.

모든 보고서는 reports에 저장한다.

모든 템플릿은 templates에 저장한다.

문서를 중복 저장하지 않는다.

---

# 14. Future Expansion

향후 다음 디렉터리를 추가할 수 있다.

```text
knowledge-base/

plugins/

automation/

ci/

benchmarks/

translations/

datasets/
```

---

# 15. Recommended Repository Structure

```text
VSkript-AI-Engineering-Kit/

docs/

prompts/

templates/

reports/

examples/

checklists/

assets/

scripts/
```

---

# 16. Quality Checklist

□ Root 구조 준수

□ Prompt 분리

□ Template 분리

□ Reports 분리

□ Examples 포함

□ Assets 분리

□ Scripts 분리

---

# 17. Document Information

| Item | Value |
|------|-------|
| Document | DIRECTORY.md |
| Version | 1.0 |
| Status | Draft |
| Owner | VSkript AI Engineering Kit |

---

# Summary

DIRECTORY.md는 VSkript AI Engineering Kit의 표준 디렉터리 구조를 정의한다.

모든 문서, 프롬프트, 예제 및 보고서는 본 구조를 기반으로 관리되며, 새로운 기능은 기존 구조를 유지하면서 확장하는 것을 원칙으로 한다.
