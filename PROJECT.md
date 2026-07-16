# PROJECT.md

# VSkript AI Engineering Kit

> A standardized AI engineering framework for developing, analyzing, verifying, and maintaining the VSkript ecosystem.

---

# 1. Project Overview

## Introduction

VSkript AI Engineering Kit는 VSkript 및 VSkript Database 프로젝트를 대상으로 하는 표준 AI 개발 프레임워크입니다.

본 프로젝트는 단순한 프롬프트 모음집(Prompt Collection)이 아니라, AI와 협업하여 프로젝트를 분석하고 개발하며 검증하는 전 과정을 표준화하기 위한 엔지니어링 시스템입니다.

이 프레임워크는 GPT, Claude, Gemini, Codex, Cursor, Windsurf, Copilot 등 다양한 AI 모델에서 동일한 수준의 품질과 일관된 결과를 얻는 것을 목표로 합니다.

---

# 2. Vision

AI를 소프트웨어 개발의 보조 도구가 아닌 **표준 개발 파트너(Standard Engineering Partner)** 로 활용할 수 있는 개발 환경을 구축한다.

궁극적으로는 VSkript 프로젝트뿐만 아니라 다른 오픈소스 프로젝트에도 적용 가능한 AI Engineering Framework로 확장하는 것을 목표로 한다.

---

# 3. Mission

본 프로젝트의 핵심 목표는 다음과 같다.

- 프로젝트 분석 표준화
- 아키텍처 검토 표준화
- 기능 구현 절차 표준화
- 코드 검증 자동화
- 코드 감사(Code Audit) 체계 구축
- 데이터베이스 설계 표준화
- AI 보고서 품질 향상
- AI 결과의 재현성 확보

---

# 4. Project Goals

## Primary Goals

- AI가 프로젝트 전체를 이해한 후 작업하도록 한다.
- 분석 결과의 신뢰성을 높인다.
- 코드 기반(Evidence-Based) 분석을 수행한다.
- 추측에 의존하지 않는 개발 환경을 구축한다.
- 기능 추가 및 리팩터링 절차를 표준화한다.

---

## Secondary Goals

- 프로젝트 문서를 자동 생성할 수 있도록 지원한다.
- AI 결과물의 일관성을 유지한다.
- 신규 참여자의 프로젝트 이해를 돕는다.
- 유지보수 비용을 절감한다.

---

# 5. Scope

## Included

본 프로젝트는 다음 영역을 포함한다.

### Analysis

- Project Analysis
- Architecture Review
- Dependency Analysis
- Performance Analysis

### Development

- Feature Implementation
- Refactoring
- Database Architecture
- DataPack System
- Performance Optimization

### Verification

- Static Verification
- Functional Verification
- Runtime Test
- Regression Test
- Code Audit
- Security Review
- Final Report

### Documentation

- Prompt Standards
- Templates
- Guidelines
- Checklists
- Workflow Documentation

---

## Excluded

다음은 본 프로젝트의 범위에 포함되지 않는다.

- 실제 VSkript 기능 개발
- VSCode Extension 구현
- SkriptHub 데이터 제공
- 플러그인 개발
- 프로젝트 배포 자동화 구현

단, 이러한 작업을 지원하기 위한 문서와 프롬프트는 포함한다.

---

# 6. Target Users

본 프로젝트는 다음 사용자를 대상으로 한다.

- VSkript Maintainers
- VSkript Contributors
- Extension Developers
- Database Maintainers
- AI-assisted Developers
- Code Reviewers
- Technical Writers

---

# 7. Supported AI Platforms

본 프로젝트는 특정 AI 모델에 종속되지 않는다.

지원 대상 예시

- ChatGPT
- Claude
- Gemini
- Codex
- Cursor
- Windsurf
- GitHub Copilot

---

# 8. Design Principles

모든 문서와 프롬프트는 아래 원칙을 따른다.

## Evidence First

모든 결론은 실제 프로젝트 구조와 코드에 근거해야 한다.

---

## No Guessing

추측으로 기능이나 구조를 가정하지 않는다.

---

## Project-Oriented

항상 프로젝트 전체를 먼저 이해한 뒤 작업한다.

---

## Maintainability

장기 유지보수를 고려한 설계를 우선한다.

---

## Extensibility

새로운 기능, 모듈, 데이터팩, 문서가 쉽게 추가될 수 있도록 설계한다.

---

## Reusability

다른 프로젝트에도 재사용할 수 있도록 범용성을 고려한다.

---

# 9. Expected Deliverables

본 프로젝트는 다음 산출물을 생성한다.

## Documentation

- README
- Architecture Documents
- Development Guides
- Workflow Guides
- Standards
- Templates

## Prompt Library

- Analysis Prompts
- Development Prompts
- Verification Prompts

## Checklists

- Runtime Checklist
- Release Checklist
- Code Review Checklist

## Templates

- Issue Report
- Verification Report
- Final Report

---

# 10. Success Criteria

다음 조건을 만족하면 프로젝트가 성공한 것으로 판단한다.

- AI가 프로젝트를 올바르게 분석할 수 있다.
- 동일한 입력에서 일관된 결과를 생성한다.
- 추측 없는 코드 기반 분석이 가능하다.
- 프로젝트 참여자가 동일한 워크플로우를 사용할 수 있다.
- 문서와 프롬프트가 유지보수 가능한 구조를 가진다.

---

# 11. Future Roadmap

## Version 1.x

- Prompt Library 구축
- Documentation 구축
- Workflow 정립

## Version 2.x

- Prompt Automation
- CI Integration
- Validation Automation

## Version 3.x

- AI Agent Integration
- Knowledge Base
- Continuous Learning

---

# 12. License

프로젝트의 라이선스는 별도 LICENSE 문서를 따른다.

---

# 13. Document Information

| Item | Value |
|------|-------|
| Document | PROJECT.md |
| Version | 1.0 |
| Status | Draft |
| Owner | VSkript AI Engineering Kit |
| Last Updated | YYYY-MM-DD |

---

# 14. Summary

VSkript AI Engineering Kit는 AI를 활용한 프로젝트 분석, 개발, 검증, 유지보수 과정을 표준화하기 위한 엔지니어링 프레임워크이다.

본 프로젝트는 단순한 프롬프트 모음이 아닌, 프로젝트 전반에 걸친 AI 협업 표준을 제공하는 것을 목표로 한다.
