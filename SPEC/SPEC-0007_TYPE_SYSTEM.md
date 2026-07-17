# SPEC-0007_TYPE_SYSTEM.md

# VSkript Type System Specification

> Common Type System Standard

Specification: SPEC-0007

Version: 1.0

Status: Draft

---

# 1. Purpose

본 문서는 VSkript Database에서 사용하는 공통 Type System을 정의한다.

모든 Grammar, Pattern, Parser, Builder, Validator, Generator, VSCode Extension 및 Language Server는 본 명세를 기반으로 Type을 해석하고 처리해야 한다.

Type System은 Skript 문법의 의미를 정의하는 핵심 요소이며, 자동 완성, Hover, Definition, Diagnostics 및 정적 분석의 기반이 된다.

---

# 2. Objectives

Type System의 목표

- 공통 타입 정의
- 타입 검증 표준화
- 타입 추론 지원
- 타입 호환성 정의
- 자동 완성 지원
- Hover 및 Definition 지원
- 향후 사용자 정의 타입 확장 지원

---

# 3. Type Categories

Type은 다음 범주로 구분한다.

| Category | Description |
|----------|-------------|
| Primitive | 기본 타입 |
| Object | Bukkit/Paper 객체 |
| Collection | 컬렉션 |
| Nullable | Nullable 타입 |
| Generic | Generic 타입 |
| Alias | 별칭 타입 |
| Custom | 사용자 정의 타입 |

---

# 4. Primitive Types

기본 제공 타입

```
string
number
boolean
timespan
date
time
color
text
```

---

# 5. Object Types

Minecraft 객체

```
player
offlineplayer
entity
livingentity
world
location
vector
chunk
block
biome
inventory
itemtype
itemstack
enchantment
potion
sound
particle
uuid
```

---

# 6. Collection Types

컬렉션 타입

```
players

entities

strings

numbers

locations
```

Collection은 동일 타입의 목록이다.

---

# 7. Nullable Types

Nullable은

```
?
```

를 사용한다.

예

```
player?

entity?

location?
```

Null 허용을 의미한다.

---

# 8. Generic Types

Generic은

```
type<T>
```

형식을 사용한다.

예

```
list<string>

list<player>

map<string, number>

set<entity>
```

---

# 9. Alias Types

Alias는 동일 타입의 다른 이름이다.

예

```
int

↓

number
```

```
bool

↓

boolean
```

Alias는 Registry에서 관리한다.

---

# 10. Custom Types

애드온은 새로운 Type을 추가할 수 있다.

예

```
nbt

bossbar

advancement

region

customitem
```

모든 Custom Type은 Registry에 등록되어야 한다.

---

# 11. Type Identifier

모든 Type은 고유 ID를 가진다.

예

```json
{
  "id": "player",
  "category": "object"
}
```

---

# 12. Type Definition

Type은 다음 구조를 가진다.

```json
{
  "id": "player",
  "displayName": "Player",
  "category": "object",
  "nullable": false,
  "collection": false
}
```

---

# 13. Type Compatibility

Type 간 호환성을 정의한다.

예

```
livingentity

↑

player

↑

offlineplayer
```

Builder는 호환성을 고려해야 한다.

---

# 14. Type Hierarchy

예시

```
object

├── entity

│   ├── livingentity

│   │   ├── player

│   │   ├── villager

│   │   └── monster

│   └── projectile

└── block
```

---

# 15. Type Conversion

자동 변환 가능한 타입

| From | To |
|------|----|
| int | number |
| float | number |
| player | entity |
| entity | livingentity (조건부) |
| string | text |

변환 규칙은 Validator가 검증한다.

---

# 16. Type Inference

Parser는 가능한 경우 타입을 추론한다.

예

```
player's location

↓

location
```

```
victim

↓

entity
```

---

# 17. Type Validation

Validator는 다음을 검사한다.

- Unknown Type
- Duplicate Type
- Invalid Alias
- Circular Alias
- Invalid Generic
- Invalid Nullable

---

# 18. Pattern Integration

Pattern은 Type을 참조한다.

예

```
spawn %entity%
```

↓

```
entity
```

↓

Registry Type

---

# 19. Completion Integration

Type 기반 자동완성

예

```
teleport

↓

%player%

↓

Player Suggestions
```

---

# 20. Hover Integration

Hover는 Type 정보를 표시한다.

예

```
player

↓

Player Object

Represents an online player.
```

---

# 21. Definition Integration

Definition Provider는 Type Definition으로 이동할 수 있다.

```
player

↓

Type Definition

↓

Grammar
```

---

# 22. Registry Integration

모든 Type은 Registry에서 관리된다.

예

```json
{
  "types": [
    "player",
    "entity",
    "world"
  ]
}
```

---

# 23. Extension Rules

Extension은

- Hover
- Completion
- Diagnostics
- Definition

생성 시 Type 정보를 사용한다.

---

# 24. Builder Rules

Builder는

- Type 중복 제거
- Alias 생성
- Registry 생성
- Dependency 연결

을 수행한다.

---

# 25. Future Extensions

향후 지원

- Union Types
- Intersection Types
- Type Constraints
- Enum Types
- Template Types
- Generic Constraints
- User Type Packages

---

# 26. Summary

Type System은 VSkript Database의 모든 문법 요소가 공유하는 공통 타입 체계를 정의한다.

모든 Parser, Builder, Validator 및 Extension은 본 명세를 기반으로 타입을 해석하고 검증해야 한다.

---

# 27. Related Specifications

- SPEC-0006_PATTERN_SYNTAX.md
- SPEC-0008_JSON_SCHEMA_GUIDE.md
- SPEC-0010_DEPENDENCY_RESOLUTION.md

---

# 28. Referenced RFC

- RFC-0003_GRAMMAR_SCHEMA.md
- RFC-0007_BUILDER_SPECIFICATION.md
- RFC-0008_VALIDATOR_SPECIFICATION.md

---

# 29. Document Information

| Item | Value |
|------|-------|
| Specification | SPEC-0007 |
| Document | TYPE_SYSTEM.md |
| Version | 1.0 |
| Status | Draft |
| Owner | VSkript Database |
| Last Updated | YYYY-MM-DD |