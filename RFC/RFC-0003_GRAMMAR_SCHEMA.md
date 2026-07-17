# RFC-0003_GRAMMAR_SCHEMA.md

# VSkript Grammar Schema

> Grammar JSON Specification (RFC-0003)

Version: 1.0

Status: Draft

---

# 1. Purpose

본 RFC는 VSkript Database에서 사용하는 모든 문법(JSON) 데이터의 공통 구조를 정의한다.

모든 DataPack은 동일한 Grammar Schema를 사용해야 하며,
Builder, Validator, Generator, VSCode Extension 및 Language Server는 본 명세를 기준으로 데이터를 생성하고 처리한다.

---

# 2. Objectives

Grammar Schema는 다음 목표를 가진다.

- 일관된 JSON 구조
- 확장 가능한 데이터 모델
- 빠른 검색
- 다국어 분리
- 버전 관리
- LSP 친화적 구조

---

# 3. Supported Grammar Types

각 DataPack은 필요한 Grammar 파일만 포함한다.

```
events.json

effects.json

conditions.json

expressions.json

functions.json

sections.json

classes.json

types.json
```

---

# 4. Common Object Structure

모든 Grammar Entry는 동일한 구조를 사용한다.

```json
{
  "id": "",
  "type": "",
  "name": "",
  "since": "",
  "deprecated": false,
  "patterns": [],
  "parameters": [],
  "returns": null,
  "examples": [],
  "documentation": "",
  "localeKey": ""
}
```

---

# 5. Required Fields

| Field | Required |
|--------|----------|
| id | ✔ |
| type | ✔ |
| name | ✔ |
| patterns | ✔ |
| localeKey | ✔ |

---

# 6. id

Grammar의 고유 ID.

규칙

- 소문자
- kebab-case
- 변경 금지

예

```
spawn-entity

teleport-player

loop-blocks
```

---

# 7. type

Grammar 종류.

허용 값

```
event

effect

condition

expression

function

section

class

type
```

---

# 8. name

내부 식별 이름.

예

```
Spawn Entity
```

---

# 9. since

최초 지원 버전.

예

```
2.8

2.9

3.0
```

---

# 10. deprecated

Deprecated 여부.

```
true

false
```

---

# 11. Patterns

실제 Skript 문법.

예

```json
[
    "spawn %entity%",
    "spawn %entity% at %location%"
]
```

Pattern은 자동완성과 Hover에서 사용된다.

---

# 12. Parameters

모든 Parameter를 정의한다.

예

```json
[
    {
        "name":"entity",
        "type":"entity",
        "required":true
    },
    {
        "name":"location",
        "type":"location",
        "required":false
    }
]
```

---

# 13. Return Type

Expression과 Function만 사용한다.

```json
{
    "type":"number"
}
```

Effect/Event는 null.

---

# 14. Examples

예제 코드.

```json
[
    "spawn zombie",
    "spawn pig at player"
]
```

---

# 15. Documentation

설명 문자열은 Locale Key를 사용한다.

Grammar에는 실제 번역을 저장하지 않는다.

예

```
grammar.spawn.description
```

---

# 16. Locale Key

Grammar는 Locale Key만 가진다.

```
grammar.spawn

grammar.teleport

grammar.loop
```

실제 번역은 locale/*.json에 저장한다.

---

# 17. Search Index

Builder는 다음 항목을 색인한다.

- id
- name
- patterns
- aliases

---

# 18. Aliases

Grammar는 Alias를 가질 수 있다.

```json
[
    "tp",
    "teleport"
]
```

---

# 19. Tags

검색용 Tag.

```json
[
    "entity",
    "movement",
    "player"
]
```

---

# 20. Categories

Grammar 분류.

예

```
Entity

Player

Inventory

Math

String

World

File
```

---

# 21. References

관련 Grammar.

```json
[
    "teleport",
    "move"
]
```

---

# 22. Validation Rules

Grammar는 반드시

- Unique ID
- Valid Pattern
- Valid Parameter
- Locale Key 존재

를 만족해야 한다.

---

# 23. Builder Rules

Builder는

- Pattern Parsing
- Parameter 생성
- Locale Key 생성

을 자동 수행한다.

---

# 24. Validator Rules

검사 항목

- Duplicate Pattern
- Duplicate ID
- Invalid Parameter
- Empty Pattern
- Invalid Return Type

---

# 25. Extension Usage

Language Server는

Grammar JSON을 이용하여

- Completion
- Hover
- Signature Help
- Definition
- Documentation

을 제공한다.

---

# 26. Performance

Grammar는

- Lazy Loading
- Memory Cache
- Fast Lookup

을 지원해야 한다.

---

# 27. Future Extensions

향후 추가

- Semantic Tokens
- AST Metadata
- Snippet Template
- Code Actions
- AI Hint Metadata
- Deprecation Message

---

# 28. Example

```json
{
  "id":"spawn-entity",

  "type":"effect",

  "name":"Spawn Entity",

  "since":"2.8",

  "deprecated":false,

  "patterns":[
      "spawn %entity%",
      "spawn %entity% at %location%"
  ],

  "parameters":[
      {
          "name":"entity",
          "type":"entity",
          "required":true
      },
      {
          "name":"location",
          "type":"location",
          "required":false
      }
  ],

  "returns":null,

  "examples":[
      "spawn zombie"
  ],

  "localeKey":"grammar.spawn-entity"
}
```

---

# 29. Summary

Grammar Schema는 VSkript Database의 핵심 데이터 모델이다.

모든 문법은 동일한 구조를 사용하며,
Builder는 이를 생성하고,
Validator는 이를 검증하며,
Language Server는 이를 기반으로 자동완성, Hover, Definition, Signature Help 및 기타 LSP 기능을 제공한다.

---

# 30. Document Information

| Item | Value |
|------|-------|
| RFC | RFC-0003 |
| Document | GRAMMAR_SCHEMA.md |
| Version | 1.0 |
| Status | Draft |
| Owner | VSkript Database |
| Last Updated | YYYY-MM-DD |