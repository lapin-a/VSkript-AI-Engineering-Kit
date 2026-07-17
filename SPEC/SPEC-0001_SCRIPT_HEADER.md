# SPEC-0001_SCRIPT_HEADER.md

# VSkript Script Header Specification

> Script Metadata Standard

Specification: SPEC-0001

Version: 1.0

Status: Draft

---

# 1. Purpose

본 문서는 VSkript Script Header의 공식 문법과 동작 규칙을 정의한다.

Script Header는 Skript 파일 상단에 위치하는 메타데이터 블록으로, 프로젝트 설정, 애드온 의존성, 언어 설정 및 VSkript Database와의 연동 정보를 제공한다.

Script Header는 실행되는 Skript 코드에는 영향을 주지 않으며, VSCode Extension과 VSkript Toolchain에서만 해석된다.

---

# 2. Objectives

Script Header의 목적

- 프로젝트 메타데이터 제공
- 필요한 애드온 선언
- 최소 지원 버전 지정
- Database 버전 지정
- 언어 설정
- 자동 DataPack 다운로드
- Builder 및 Validator 지원

---

# 3. Location

Script Header는 반드시 파일의 가장 첫 부분에 위치해야 한다.

예시

```vb
# VSkript:
# Requires: Skript >=2.9
# Requires: SkBee >=3.12

command /hello:
    trigger:
        send "Hello"
```

Header 아래에는 빈 줄 하나를 두는 것을 권장한다.

---

# 4. Header Identifier

모든 Header는 다음 식별자로 시작한다.

```vb
# VSkript:
```

이 식별자가 없으면 Extension은 Header를 무시한다.

---

# 5. Syntax

기본 형식

```vb
# Key: Value
```

예

```vb
# Language: ko
```

---

# 6. Supported Keys

| Key | Required | Description |
|------|----------|-------------|
| Requires | No | 필요한 애드온 |
| Language | No | 기본 언어 |
| Database | No | Database Version |
| Target | No | 서버 플랫폼 |
| Version | No | 프로젝트 버전 |
| Author | No | 작성자 |
| Description | No | 프로젝트 설명 |
| Experimental | No | 실험 기능 사용 여부 |
| Ignore | No | 특정 검사 제외 |

---

# 7. Requires

필요한 애드온을 선언한다.

예

```vb
# Requires: Skript >=2.9
# Requires: SkBee >=3.12
# Requires: SkQuery
```

동일한 Key를 여러 번 사용할 수 있다.

---

# 8. Version Expression

지원 연산자

```
=
>
>=
<
<=
^
~
```

예

```
>=3.0
^2.9
~3.1
```

---

# 9. Language

기본 Locale을 지정한다.

예

```vb
# Language: ko
```

ISO Language Code를 사용한다.

예

```
en
ko
ja
zh-CN
```

---

# 10. Database

필요한 Database 버전을 지정한다.

예

```vb
# Database: ^1.0
```

---

# 11. Target

지원 플랫폼을 지정한다.

예

```vb
# Target: Paper
```

지원 값

- Bukkit
- Spigot
- Paper
- Folia
- Purpur

---

# 12. Version

프로젝트 버전

예

```vb
# Version: 1.2.0
```

Semantic Versioning을 사용한다.

---

# 13. Author

작성자 정보

```vb
# Author: Lapin
```

---

# 14. Description

프로젝트 설명

```vb
# Description: Example Project
```

---

# 15. Experimental

실험 기능 사용 여부

```vb
# Experimental: true
```

기본값은 false이다.

---

# 16. Ignore

특정 Validator 규칙을 비활성화한다.

예

```vb
# Ignore: deprecated
# Ignore: unused-import
```

동일한 Key를 여러 번 사용할 수 있다.

---

# 17. Parsing Rules

- Key는 대소문자를 구분하지 않는다.
- Value는 Trim 처리한다.
- 알 수 없는 Key는 무시한다.
- Header 종료 후에는 일반 주석으로 처리한다.

---

# 18. Validation Rules

Validator는 다음을 검사한다.

- Header Identifier 존재
- Key 형식
- Version 형식
- Language 코드
- Target 값
- Duplicate Key 허용 여부

---

# 19. Processing Flow

```
Read File
    ↓
Detect Header
    ↓
Parse Keys
    ↓
Validate
    ↓
Create Metadata
    ↓
Plugin Detection
```

---

# 20. Example

```vb
# VSkript:
# Requires: Skript >=2.9
# Requires: SkBee >=3.12
# Language: ko
# Database: ^1.0
# Target: Paper
# Version: 1.0.0
# Author: Lapin
# Description: Example Project

command /example:
    trigger:
        send "Hello"
```

---

# 21. Future Extensions

향후 추가 예정

- License
- Homepage
- Repository
- Tags
- Category
- Documentation URL
- Minimum Java Version

---

# 22. Summary

Script Header는 VSkript 프로젝트의 공식 메타데이터 규격이다.

VSCode Extension, Builder, Validator 및 Plugin Detection은 이 명세를 기준으로 프로젝트를 분석하며, 필요한 DataPack을 자동으로 선택하고 프로젝트 설정을 해석한다.

---

# 23. Document Information

| Item | Value |
|------|-------|
| Specification | SPEC-0001 |
| Document | SCRIPT_HEADER.md |
| Version | 1.0 |
| Status | Draft |
| Owner | VSkript Database |
| Last Updated | YYYY-MM-DD |