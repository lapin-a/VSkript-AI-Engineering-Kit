# SPEC-0006_PATTERN_SYNTAX.md

# VSkript Pattern Syntax Specification

> Grammar Pattern Standard

Specification: SPEC-0006

Version: 1.0

Status: Draft

---

# 1. Purpose

본 문서는 VSkript Database에서 사용하는 Grammar Pattern의 공식 문법을 정의한다.

Grammar Pattern은 Skript 구문의 구조를 표현하며, Builder, Validator, Generator, VSCode Extension 및 Language Server에서 공통적으로 사용된다.

본 명세는 Pattern Parser가 반드시 지원해야 하는 문법을 정의한다.

---

# 2. Objectives

Pattern Syntax의 목표

- 문법 표현 표준화
- Parser 일관성 확보
- 자동 검증 지원
- Hover 및 Completion 생성
- Definition Lookup 지원
- 향후 확장성 확보

---

# 3. Pattern Structure

Pattern은 문자열로 표현된다.

예시

```
spawn %entity%
```

```
teleport %player% to %location%
```

---

# 4. Literal

Literal은 그대로 입력되어야 하는 문자열이다.

예시

```
spawn

teleport

loop

send
```

---

# 5. Placeholder

Placeholder는 타입을 나타낸다.

형식

```
%type%
```

예시

```
%player%

%entity%

%location%

%number%
```

---

# 6. Optional Section

선택 입력은 대괄호를 사용한다.

```
[optional]
```

예시

```
spawn %entity% [at %location%]
```

---

# 7. Alternatives

여러 선택지는 괄호와 파이프를 사용한다.

```
(one|two|three)
```

예시

```
(set|add|remove)
```

---

# 8. Nested Patterns

중첩은 허용된다.

예시

```
[(to|at) %location%]
```

---

# 9. Multiple Alternatives

예시

```
(send|broadcast) %string%
```

---

# 10. Expression Types

지원 타입

```
player

entity

block

item

location

world

vector

string

number

boolean

offlineplayer

inventory

chunk

uuid
```

새로운 타입은 Registry를 통해 추가할 수 있다.

---

# 11. List Types

리스트 타입은

```
%players%
```

처럼 복수형을 사용한다.

예시

```
loop %players%
```

---

# 12. Nullable Types

선택 가능한 타입은

```
%player?%
```

로 표현한다.

---

# 13. Repeating Elements

반복은

```
...
```

를 사용한다.

예시

```
%string%, ...
```

---

# 14. Escaping

예약 문자는 Escape한다.

```
\%

\[

\]

\(

\)

\|
```

---

# 15. Pattern Identifier

모든 Pattern은 ID를 가진다.

예시

```json
{
  "id":"effect.spawn",
  "pattern":"spawn %entity%"
}
```

---

# 16. Pattern Priority

동일한 Pattern이 여러 개 존재할 경우

Priority를 사용한다.

```json
{
  "priority":100
}
```

높을수록 먼저 검사한다.

---

# 17. Pattern Validation

Validator는 다음을 검사한다.

- Placeholder Pair
- Optional Pair
- Parentheses Pair
- Escape
- Unknown Type
- Invalid Syntax

---

# 18. Pattern Parsing Flow

```
Pattern

↓

Lexer

↓

Tokens

↓

Parser

↓

AST

↓

Validator
```

---

# 19. AST Example

Pattern

```
spawn %entity% [at %location%]
```

↓

AST

```
Literal

Placeholder

Optional

Literal

Placeholder
```

---

# 20. Completion

Pattern은 Completion을 생성한다.

예시

```
spawn
```

↓

Suggestion

```
spawn %entity%
```

---

# 21. Hover

Hover는 Pattern과 Description을 표시한다.

```
spawn %entity%
```

↓

```
Spawn an entity.
```

---

# 22. Definition

Pattern은 Definition Provider와 연결된다.

```
Pattern

↓

Definition

↓

Grammar JSON
```

---

# 23. Future Extensions

향후 지원 예정

- Generic Types
- Union Types
- Function Signatures
- Named Parameters
- Type Constraints
- Regular Expression Support

---

# 24. Summary

Pattern Syntax는 VSkript Database에서 사용하는 모든 Grammar Pattern의 공식 문법을 정의한다.

모든 Parser와 Builder는 본 명세를 기반으로 Pattern을 처리해야 하며, 동일한 결과를 생성해야 한다.

---

# 25. Related Specifications

- SPEC-0007_TYPE_SYSTEM.md
- SPEC-0008_JSON_SCHEMA_GUIDE.md

---

# 26. Referenced RFC

- RFC-0003_GRAMMAR_SCHEMA.md
- RFC-0007_BUILDER_SPECIFICATION.md
- RFC-0008_VALIDATOR_SPECIFICATION.md

---

# 27. Document Information

| Item | Value |
|------|-------|
| Specification | SPEC-0006 |
| Document | PATTERN_SYNTAX.md |
| Version | 1.0 |
| Status | Draft |
| Owner | VSkript Database |
| Last Updated | YYYY-MM-DD |