# AI_AUTHORING_GUIDE.md

# AI Authoring Guide

> VSkript AI Engineering Kit

Version: 1.0

---

# Part 1. Overview

## 1. Purpose

본 문서는 AI가 VSkript AI Engineering Kit의 모든 문서를 작성할 때 따라야 하는 공식 Authoring 기준을 정의한다.

AI는 본 문서를 기준으로 문서를 작성하며, 모든 산출물은 동일한 품질과 구조를 유지해야 한다.

---

## 2. Scope

본 문서는 다음 문서 작성에 적용된다.

* Core Documents
* Specifications
* RFC
* ADR
* Contract Documents
* Guides
* Templates
* Reports
* Prompt Documents
* Checklists

---

## 3. Authoring Philosophy

모든 문서는 다음 철학을 기반으로 작성한다.

* Specification First
* Documentation First
* Evidence First
* Canonical First
* Consistency First

문서는 구현보다 먼저 존재하며, 구현의 기준이 된다.

---

## 4. Authoring Principles

모든 문서는 다음 원칙을 따른다.

* Evidence First
* No Guessing
* Single Source of Truth
* Canonical Model
* Explicit Dependencies
* Stable Structure
* Deterministic Output

---

# Part 2. Document Lifecycle

## 5. Document Types

프로젝트는 다음 문서 유형을 사용한다.

* Core Documents
* Specifications
* Contracts
* RFC
* ADR
* Guides
* Templates
* Reports
* Examples

---

## 6. Document Lifecycle

모든 문서는 다음 생명주기를 따른다.

```text
Draft

↓

Review

↓

Revision

↓

Approval

↓

Published

↓

Maintenance
```

---

## 7. Draft vs Approved

Draft 문서는 변경될 수 있다.

Approved 문서는 공식 Reference로 사용된다.

AI는 Draft와 Approved 상태를 혼동해서는 안 된다.

---

## 8. Versioning Policy

모든 문서는 Version을 가진다.

Version 변경 시 다음 정보를 함께 관리한다.

* Version
* Status
* Updated Date
* Change Summary

---

# Part 3. Writing Rules

## 9. Canonical Structure

모든 문서는 Canonical Structure를 가져야 한다.

동일한 종류의 문서는 항상 동일한 구조를 유지해야 한다.

---

## 10. Canonical Composition

Composition을 정의하는 문서는 Canonical Composition을 명확하게 정의해야 한다.

Structure와 Composition은 서로 모순되어서는 안 된다.

---

## 11. Section Numbering

모든 Section 번호는 문서 내에서 유일해야 한다.

번호 중복은 허용되지 않는다.

---

## 12. Naming Rules

모든 명칭은 CORE_GLOSSARY를 따른다.

동일한 개념에 여러 이름을 사용해서는 안 된다.

---

## 13. Terminology Usage

새로운 용어를 사용할 경우 먼저 CORE_GLOSSARY에 등록해야 한다.

등록되지 않은 용어는 공식 문서에서 사용하지 않는다.

---

## 14. Cross References

문서 간 참조는 명시적으로 작성한다.

Cross Reference는 실제 존재하는 문서만 참조할 수 있다.

---

## 15. Cross Contract Dependencies

Contract를 참조하는 문서는 Dependency를 명시해야 한다.

존재하지 않는 Contract를 참조해서는 안 된다.

---

## 16. Relationship Model

Relationship Diagram은 프로젝트 전체에서 하나의 Canonical Model만 사용한다.

동일한 개념에 여러 구조를 정의해서는 안 된다.

---

# Part 4. Consistency Rules

## 17. Single Source of Truth

동일한 정보는 하나의 공식 문서만 가진다.

다른 문서는 해당 정보를 복사하지 않고 참조한다.

---

## 18. Canonical First

Canonical Structure가 존재하는 경우 이를 우선한다.

요약이나 예시는 Canonical Definition과 충돌해서는 안 된다.

---

## 19. No Duplicate Definitions

동일한 정의를 여러 방식으로 작성해서는 안 된다.

동일한 내용은 하나의 공식 정의만 유지한다.

---

## 20. Cross Document Consistency

모든 문서는 프로젝트의 다른 문서와 일관성을 유지해야 한다.

문서 간 충돌이 발생하면 수정 후 다시 검토한다.

---

## 21. Layer Consistency

모든 Layer는 CORE_DESIGN_PRINCIPLES에서 정의한 구조를 따른다.

Layer의 방향과 역할은 변경하지 않는다.

---

## 22. Runtime Independence

문서는 특정 Runtime 구현에 종속되어서는 안 된다.

문서는 개념과 모델을 설명해야 하며, 구현은 별도의 문서에서 정의한다.

---

# Part 5. AI Authoring Rules

## 23. Evidence First

모든 내용은 확인 가능한 근거를 기반으로 작성한다.

추측이나 경험에 의존해서는 안 된다.

---

## 24. No Guessing

확인되지 않은 내용은 작성하지 않는다.

불확실한 내용은 명시적으로 표시하거나 제외한다.

---

## 25. Specification First

Specification은 프로젝트의 공식 Source of Truth이다.

구현보다 Specification을 우선한다.

---

## 26. Contract First

Contract는 프로젝트의 공식 데이터 모델이다.

Contract를 변경하는 경우 관련 Specification과 함께 검토해야 한다.

---

## 27. Documentation First

구현 전에 문서를 작성한다.

문서는 구현의 기준이 된다.

---

## 28. Stable Ordering

목록과 구조는 항상 동일한 순서를 유지한다.

Generator와 AI는 임의로 순서를 변경해서는 안 된다.

---

## 29. Deterministic Output

동일한 입력은 항상 동일한 문서를 생성해야 한다.

문서 생성 결과는 재현 가능해야 한다.

---

# Part 6. Validation

## 30. Self Review Checklist

문서를 제출하기 전에 다음 항목을 확인한다.

* Canonical Structure 확인
* 용어 일관성 확인
* Cross Reference 확인
* 문서 번호 확인
* Version 확인
* Document Information 확인

---

## 31. Common Mistakes

대표적인 오류는 다음과 같다.

* 중복 정의
* 번호 중복
* 존재하지 않는 문서 참조
* Canonical Structure 불일치
* Glossary 미등록 용어 사용

---

## 32. Review Preparation

모든 문서는 Review 전에 자체 검토를 수행한다.

검토 결과를 반영한 후 Review를 요청한다.

---

## 33. Approval Criteria

문서는 다음 조건을 만족해야 승인된다.

* 구조 검증 완료
* 의미 검증 완료
* 문서 간 일관성 확인
* Self Review 완료

---

## 34. Definition of Done

문서는 다음 조건을 모두 만족해야 완료된다.

* Canonical Structure 정의
* 문서 간 일관성 확보
* Review 완료
* Approval 완료
* Version 기록 완료

---

# Document Information

| Item     | Value                      |
| -------- | -------------------------- |
| Document | AI_AUTHORING_GUIDE.md      |
| Version  | 1.0                        |
| Status   | Draft                      |
| Owner    | VSkript AI Engineering Kit |

---

# Summary

AI_AUTHORING_GUIDE.md는 VSkript AI Engineering Kit에서 AI가 모든 문서를 작성할 때 따라야 하는 공식 작성 기준을 정의한다.

모든 문서는 본 가이드에 따라 작성되며, Canonical Structure, 용어 일관성, 문서 간 관계, Versioning 정책을 준수해야 한다.
