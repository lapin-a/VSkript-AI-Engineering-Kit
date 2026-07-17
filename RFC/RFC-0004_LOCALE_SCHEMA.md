# RFC-0004_LOCALE_SCHEMA.md

# VSkript Locale Schema

> Locale System Specification (RFC-0004)

Version: 1.0

Status: Draft

---

# 1. Purpose

본 RFC는 VSkript Database에서 사용하는 Locale 시스템의 구조와 규칙을 정의한다.

Locale 시스템은 문법 데이터와 사용자에게 표시되는 문자열을 분리하여,
다국어 지원, 번역 유지보수 및 확장성을 제공한다.

Grammar JSON에는 실제 문자열을 저장하지 않으며,
Locale Key를 통해 각 언어 파일을 참조한다.

---

# 2. Objectives

Locale 시스템의 목표

- 다국어 지원
- 문법 데이터와 번역 분리
- 쉬운 번역 관리
- 증분 번역 업데이트
- 번역 재사용
- LSP 표시 최적화

---

# 3. Directory Structure

```
locale/

en.json

ko.json

ja.json

zh-CN.json

zh-TW.json

de.json

fr.json

...
```

---

# 4. File Encoding

모든 Locale 파일은

- UTF-8
- JSON
- LF
- 2 Spaces

를 사용한다.

---

# 5. Translation Model

Grammar JSON

↓

Locale Key

↓

Locale File

↓

Translated Text

---

# 6. Locale Key Rules

Locale Key는 프로젝트 전체에서 유일해야 한다.

형식

```
grammar.spawn.description

grammar.spawn.example

grammar.teleport.description

event.onJoin.description
```

---

# 7. Key Naming Convention

형식

```
category.object.field
```

예

```
effect.spawn.description

effect.spawn.examples

effect.spawn.notes

condition.player-online.description

expression.block.location
```

---

# 8. Translation Object

예시

```json
{
    "grammar.spawn.description":"Spawn an entity.",
    "grammar.spawn.example":"spawn zombie"
}
```

---

# 9. Recommended Fields

각 Grammar는 다음 문자열을 가진다.

| Field | Required |
|--------|----------|
| description | ✔ |
| examples | ✔ |
| notes | Optional |
| warnings | Optional |
| deprecated | Optional |

---

# 10. Language Codes

ISO-639 + ISO-3166 사용.

예

```
en

ko

ja

zh-CN

zh-TW

de

fr
```

---

# 11. Default Locale

기본 언어는

```
en
```

이다.

다른 언어가 없으면 en으로 fallback한다.

---

# 12. Fallback Strategy

예

```
ko

↓

en

↓

Key
```

번역이 없으면

Locale Key를 그대로 출력하지 않고,
기본 언어를 우선 사용한다.

---

# 13. Grammar Integration

Grammar JSON

```json
{
    "localeKey":"grammar.spawn"
}
```

↓

Locale

```json
{
    "grammar.spawn.description":"Spawn an entity."
}
```

---

# 14. Placeholder Rules

Placeholder 사용 가능.

예

```
{entity}

{player}

{location}
```

예시

```
Spawn {entity} at {location}
```

---

# 15. Markdown Support

설명은 Markdown 일부를 지원한다.

허용

- Bold
- Italic
- Inline Code
- Bullet List

허용하지 않음

- HTML
- Script
- Image

---

# 16. Validation Rules

Locale는

- Duplicate Key 없음
- Empty Value 없음
- Invalid JSON 없음

을 만족해야 한다.

---

# 17. Builder Rules

Builder는

- Locale Key 생성
- 기본 en.json 생성

을 자동 수행한다.

---

# 18. Translation Workflow

```
Grammar

↓

Locale Key

↓

en.json

↓

Other Languages

↓

Validation
```

---

# 19. Incremental Translation

변경된 Locale Key만 갱신한다.

```
Old Locale

↓

Diff

↓

New Keys

↓

Update
```

---

# 20. Search

Locale는

- Key
- Value

검색을 지원해야 한다.

---

# 21. Extension Usage

VSCode Extension은

사용자 Locale을 확인한 후

```
Grammar

↓

Locale

↓

Hover

↓

Completion

↓

Documentation
```

순서로 사용한다.

---

# 22. Missing Translation

번역이 없는 경우

1. en 사용
2. Locale Key 기록
3. Translation Report 생성

---

# 23. AI Translation

향후 지원

- AI Translation
- Machine Translation Review
- Translation Suggestion

---

# 24. Quality Requirements

Locale는

- JSON Validation
- Duplicate 검사
- Missing Translation 검사
- Placeholder 검사

를 통과해야 한다.

---

# 25. Example

Grammar

```json
{
    "localeKey":"effect.spawn"
}
```

ko.json

```json
{
    "effect.spawn.description":"엔티티를 생성합니다.",
    "effect.spawn.examples":"spawn zombie"
}
```

en.json

```json
{
    "effect.spawn.description":"Spawn an entity.",
    "effect.spawn.examples":"spawn zombie"
}
```

---

# 26. Future Extensions

향후 지원

- Plural Rules
- Gender Rules
- RTL Languages
- Translation Metadata
- Community Translation Platform

---

# 27. Summary

Locale Schema는 VSkript Database의 다국어 시스템을 정의한다.

Grammar JSON은 Locale Key만 보관하며,
실제 문자열은 언어별 Locale 파일에서 관리한다.

이를 통해 문법 데이터와 번역을 독립적으로 관리하고,
증분 번역 업데이트와 다국어 확장을 효율적으로 지원한다.

---

# 28. Document Information

| Item | Value |
|------|-------|
| RFC | RFC-0004 |
| Document | LOCALE_SCHEMA.md |
| Version | 1.0 |
| Status | Draft |
| Owner | VSkript Database |
| Last Updated | YYYY-MM-DD |