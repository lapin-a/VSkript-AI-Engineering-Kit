# VSkript AI Engineering Kit
 
VSkript 및 VSkript Database 프로젝트를 대상으로, AI와 협업하여 프로젝트를 분석하고 개발하며 검증하는 전 과정을 표준화하기 위한 AI 엔지니어링 프레임워크입니다.
 
---
 
## 프로젝트 개요
 
VSkript AI Engineering Kit는 단순한 Prompt 모음집이 아니라, AI가 프로젝트를 분석·구현·검증·문서화하는 전체 과정을 표준화하기 위한 문서 기반 엔지니어링 시스템입니다.
 
- **목적**: AI가 프로젝트 전체를 이해한 상태에서 작업하도록 하고, 근거(Evidence) 기반의 분석과 재현 가능한 결과를 얻는다.
- **해결하려는 문제**: AI 모델마다 결과 품질이 달라지고, 추측에 기반한 분석이나 프로젝트 구조를 이해하지 못한 구현이 발생하는 문제를 표준 Workflow와 Rule로 해결한다.
- **대상 사용자**: VSkript Maintainers, VSkript Contributors, Extension Developers, Database Maintainers, AI-assisted Developers, Code Reviewers, Technical Writers.
- **주요 특징**: 특정 AI 모델에 종속되지 않으며, ChatGPT, Claude, Gemini, Codex, Cursor, Windsurf, GitHub Copilot 등 다양한 AI 플랫폼에서 동일한 절차와 품질 기준을 적용하는 것을 목표로 한다.

이 프로젝트는 AI가 프로젝트를 이해하고, 구현하고, 검증하고, 문서화하는 전 과정을 일관된 품질 기준으로 수행할 수 있도록 설계되었다.

---
 
## 주요 기능
 
- **표준 Workflow**: Project Analysis부터 Final Report까지 14단계로 구성된 표준 작업 절차를 정의한다.
- **Rules / AI Behavior Policy**: 모든 AI가 따라야 하는 공통 규칙(Evidence First, No Guessing 등)을 정의한다.
- **Prompt Standard**: 모든 Prompt가 따라야 하는 공통 구조(Purpose, Scope, Role 등)를 정의한다.
- **Report Standard**: 모든 보고서가 따라야 하는 공통 구조(Executive Summary, Findings, Evidence 등)를 정의한다.
- **Quality Gate**: 프로젝트 이해부터 최종 검토까지 6단계 품질 기준(Definition of Done)을 정의한다.
- **Style Guide**: 문서 작성 원칙, Markdown 규칙, 용어 사용 규칙을 정의한다.
- **Glossary**: 프로젝트 전반에서 사용하는 용어를 표준화한다.
- **Directory 표준**: 문서, Prompt, Template, Report 등을 관리하기 위한 표준 디렉터리 구조를 정의한다.
- **Template / Document Index**: 프로젝트에서 관리하는 모든 문서, Prompt, Template, Checklist의 목차를 제공한다.
- **Roadmap**: 버전별 장기 개발 계획을 정의한다.
---
 
## 프로젝트 구조
 
현재 저장소는 아래와 같이 최상위 문서 파일로 구성되어 있습니다.
 
```text
VSkript-AI-Engineering-Kit/
├── README.md              # 프로젝트 소개
├── PROJECT.md              # 프로젝트 목표, 비전, 범위 정의
├── REQUIREMENTS.md         # 기능적/비기능적 요구사항
├── WORKFLOW.md              # 표준 작업 절차(14 Phase) 정의
├── RULES.md                 # AI가 따라야 하는 공통 규칙
├── STYLE_GUIDE.md            # 문서/Prompt 작성 규칙
├── PROMPT_STANDARD.md        # Prompt 표준 구조 정의
├── REPORT_STANDARD.md        # Report 표준 구조 정의
├── QUALITY_GATE.md            # 품질 기준 및 Definition of Done
├── DIRECTORY.md               # 표준 디렉터리 구조 정의
├── TEMPLATE_LIST.md            # 전체 문서/Prompt/Template Master Index
├── ROADMAP.md                   # 버전별 장기 개발 계획
├── GLOSSARY.md                   # 표준 용어 사전
├── AI_GUIDELINES.md               # AI 행동 규칙(Behavior Policy)
├── CONTRIBUTING.md                 # 기여 가이드
└── CHANGELOG.md                     # 변경 이력
```
 
DIRECTORY.md는 `docs/`, `prompts/`, `templates/`, `reports/`, `checklists/`, `examples/`, `assets/`, `scripts/` 등을 포함하는 표준 디렉터리 구조를 정의하고 있으며, 이는 프로젝트가 향후 확장될 때 따라야 할 기준 구조입니다.
 
---
 
## Workflow
 
WORKFLOW는 14개의 Phase를 순서대로 수행하며,
각 Phase는 이전 Phase의 산출물을 입력(Input)으로 사용한다.
 
```text
Project Analysis
    ↓
Architecture Review
    ↓
Dependency Analysis
    ↓
Performance Analysis
    ↓
Implementation Planning
    ↓
Feature Implementation
    ↓
Refactoring
    ↓
Static Verification
    ↓
Functional Verification
    ↓
Runtime Test
    ↓
Regression Test
    ↓
Code Audit
    ↓
Security Review
    ↓
Final Report
```
 
- 각 Phase는 목적(Purpose), 입력(Input), 수행 작업(Tasks), 산출물(Output), 그리고 해당 Phase에 대응하는 Prompt 파일명으로 정의되어 있습니다.
- Database Architecture, DataPack System, Performance Optimization은 프로젝트 특성에 따라 선택적으로 수행하는 Development Prompt로 별도 정의되어 있습니다.
- 상황별로 권장되는 Workflow 조합(신규 기능 구현, 리팩터링, 버그 수정, 성능 개선, 데이터베이스 개편, 릴리즈 전 점검 등)이 Workflow Selection Guide에 정리되어 있습니다.
- Workflow는 모든 요구사항 충족, 정적/기능 검증 완료, Runtime Test Checklist 작성, 코드 감사 완료, 최종 보고서 작성 완료라는 Exit Criteria를 모두 만족해야 종료됩니다.
---
 
## 문서 구성
 
| 문서 | 설명 |
|------|------|
| README.md | 프로젝트 소개 |
| PROJECT.md | 프로젝트 목표, 비전, 범위 정의 |
| REQUIREMENTS.md | 기능적/비기능적 요구사항 정의 |
| WORKFLOW.md | 표준 작업 절차(Standard Workflow) 정의 |
| RULES.md | 모든 AI가 따라야 하는 공통 규칙 |
| STYLE_GUIDE.md | 문서 및 Prompt 작성 규칙 |
| PROMPT_STANDARD.md | Prompt 표준 구조 정의 |
| REPORT_STANDARD.md | Report 표준 형식 정의 |
| DIRECTORY.md | 표준 디렉터리 구조 정의 |
| TEMPLATE_LIST.md | 전체 문서/Prompt/Template Master Index |
| ROADMAP.md | 버전별 장기 개발 계획 |
| GLOSSARY.md | 표준 용어 사전 |
| QUALITY_GATE.md | 품질 기준 및 Definition of Done |
| AI_GUIDELINES.md | AI 행동 규칙(Behavior Policy) |
| CONTRIBUTING.md | 기여 가이드 |
| CHANGELOG.md | 변경 이력 |
 
---
 
## 사용 방법
 
CONTRIBUTING.md에 따르면, 작업을 시작하기 전에 아래 문서를 먼저 읽어야 합니다.
 
1. RULES.md 확인 — AI가 따라야 하는 공통 규칙을 확인한다.
2. PROJECT.md 확인 — 프로젝트 목표와 범위를 이해한다.
3. REQUIREMENTS.md 확인 — 충족해야 하는 요구사항을 확인한다.
4. WORKFLOW.md 확인 — 수행할 작업에 해당하는 Workflow 단계와 Workflow Selection Guide를 확인한다.
5. AI_GUIDELINES.md 확인 — AI의 행동 원칙(Behavior Policy)을 확인한다.
6. STYLE_GUIDE.md 확인 — 문서 작성 규칙을 확인한다.
7. QUALITY_GATE.md 확인 — 작업 완료 기준(Definition of Done)을 확인한다.
이후 WORKFLOW.md에 정의된 절차(Project Analysis → Architecture Review → … → Final Report)에 따라 작업을 진행하며, 각 단계는 REPORT_STANDARD.md에 정의된 형식으로 결과를 기록합니다.
 
---
 
## 프로젝트 특징
 
- **모델 독립성**: 특정 AI 모델에 종속되지 않고 여러 AI 플랫폼에서 동일한 절차를 적용할 수 있도록 설계되었다.
- **근거 기반 분석**: 모든 결론은 실제 코드, 설정 파일, 문서 등 확인 가능한 근거를 기반으로 해야 하며, 추측은 허용되지 않는다.
- **표준화된 절차**: 프로젝트 분석, 구현, 검증, 보고서 작성까지 전체 과정이 문서로 표준화되어 있다.
- **재사용성**: STYLE_GUIDE.md의 Reusability 원칙에 따라 특정 프로젝트에 종속되지 않도록 작성되어, VSkript 외 다른 프로젝트에도 적용 가능하도록 설계되었다(REQUIREMENTS.md NFR-005).
---
 
## 대상 사용자
 
PROJECT.md에 정의된 대상 사용자는 다음과 같습니다.
 
- VSkript Maintainers
- VSkript Contributors
- Extension Developers
- Database Maintainers
- AI-assisted Developers
- Code Reviewers
- Technical Writers
---
 
## 권장 사용 순서
 
RULES.md의 Document Priority에 따른 문서 우선순위는 다음과 같습니다.
 
1. RULES.md
2. PROJECT.md
3. REQUIREMENTS.md
4. WORKFLOW.md
5. AI_GUIDELINES.md / PROMPT_STANDARD.md / REPORT_STANDARD.md / STYLE_GUIDE.md / QUALITY_GATE.md / DIRECTORY.md / TEMPLATE_LIST.md / GLOSSARY.md
6. Prompt Files
7. Templates
8. Reports
문서 간 내용이 충돌하는 경우 RULES.md를 최우선 기준으로 판단합니다.
 
---
 
## 프로젝트 원칙
 
RULES.md와 STYLE_GUIDE.md에 정의된 핵심 원칙은 다음과 같습니다.
 
- **Evidence First**: 모든 결론은 실제 프로젝트 구조 또는 코드에 근거해야 한다.
- **No Guessing**: 추측이나 가정, 경험 기반 추론에 의존하지 않는다.
- **Project First**: 파일 하나가 아닌 프로젝트 전체를 먼저 이해한 뒤 작업한다.
- **Safety First**: 기존 기능(Completion, Hover, Signature Help, Definition, References 등)을 특별한 이유 없이 변경하지 않는다.
- **Maintainability First**: 장기 유지보수를 고려한 설계를 우선한다.
- **Transparency**: 모든 변경 사항에 대해 변경 목적, 기대 효과, 영향 범위를 설명해야 한다.
- **Consistency**: 동일한 용어와 형식을 문서 전반에서 일관되게 사용한다.

---

## Design Philosophy

### Specification First

모든 구현은 Specification을 기반으로 수행한다.

Specification은 프로젝트의 공식 Source of Truth이며,
코드와 문서는 항상 Specification과 일치해야 한다.

---
 
## License

This project does not currently define a license.

A license may be added in a future release.
