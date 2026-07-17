# DTO.md

Version: 1.0

Status: Draft

Related Documents

- RFC-0007 Builder Specification
- RFC-0008 Validator Specification
- RFC-0009 Generator Specification
- ADR-0004 Registry as Single Source of Truth
- SPEC-0009 Versioning

---

# 1. Introduction

본 문서는 VSkript Database 프로젝트에서 사용되는 모든 Data Transfer Object(DTO)를 정의한다.

DTO는 컴포넌트 간 데이터를 교환하기 위한 공통 계약(Contract)이다.

다음 컴포넌트는 모두 본 문서에서 정의한 DTO를 사용해야 한다.

- Builder
- Validator
- Generator
- Registry
- Language Server
- CLI
- Plugin Manager
- Update Manager

DTO는 구현 언어와 무관한 논리 모델(Logical Model)이며, TypeScript 인터페이스를 기준으로 기술한다.

---

# 2. Design Principles

모든 DTO는 다음 원칙을 따른다.

- Immutable
- Serializable
- Versioned
- Deterministic
- Backward Compatible (Minor Version)
- Language Independent

DTO는 비즈니스 로직을 포함하지 않는다.

---

# 3. Serialization Rules

DTO는 JSON 직렬화를 기본으로 한다.

규칙

- UTF-8 Encoding
- camelCase Property
- ISO-8601 DateTime
- Semantic Version String
- Null 대신 Optional Property 사용 권장

예시

```json
{
  "builderVersion": "1.0.0",
  "schemaVersion": "1.0.0"
}
```

---

# 4. Common Types

## ResourceId

```typescript
type ResourceId = string;
```

예시

```
skript.effect.teleport
```

---

## PluginId

```typescript
type PluginId = string;
```

---

## Namespace

```typescript
type Namespace = string;
```

---

## Version

```typescript
type Version = string;
```

Semantic Versioning을 따른다.

---

# 5. BuildContext

Builder가 생성하는 최종 결과이다.

```typescript
interface BuildContext {

    manifest: Manifest;

    registry: Registry;

    resources: Resource[];

    dependencyGraph: DependencyGraph;

    plugins: Plugin[];

    diagnostics: Diagnostic[];

}
```

BuildContext는 Validator와 Generator의 입력으로 사용된다.

---

# 6. BuildResult

```typescript
interface BuildResult {

    success: boolean;

    context: BuildContext;

    diagnostics: Diagnostic[];

    metrics: BuildMetrics;

}
```

---

# 7. ValidationResult

```typescript
interface ValidationResult {

    success: boolean;

    diagnostics: Diagnostic[];

    metrics: ValidationMetrics;

}
```

---

# 8. GeneratorResult

```typescript
interface GeneratorResult {

    success: boolean;

    outputPath: string;

    diagnostics: Diagnostic[];

    metrics: GeneratorMetrics;

}
```

---

# 9. Resource

모든 Resource의 공통 표현이다.

```typescript
interface Resource {

    id: ResourceId;

    plugin: PluginId;

    namespace: Namespace;

    type: string;

    version: Version;

    sourcePath: string;

}
```

---

# 10. Plugin

```typescript
interface Plugin {

    id: PluginId;

    name: string;

    version: Version;

    dependencies: Dependency[];

}
```

---

# 11. Manifest

```typescript
interface Manifest {

    id: string;

    version: Version;

    schemaVersion: Version;

    plugins: Plugin[];

}
```

---

# 12. Registry

```typescript
interface Registry {

    resources: Resource[];

    version: Version;

}
```

Registry는 Resource 조회의 단일 진입점이다.

---

# 13. Dependency

```typescript
interface Dependency {

    plugin: PluginId;

    version: string;

}
```

---

# 14. Diagnostic

```typescript
interface Diagnostic {

    code: string;

    severity: Severity;

    category: string;

    message: string;

    resource: ResourceId;

    location?: DiagnosticLocation;

    suggestion?: string;

}
```

---

# 15. DiagnosticLocation

```typescript
interface DiagnosticLocation {

    file: string;

    line: number;

    column: number;

    endLine: number;

    endColumn: number;

}
```

---

# 16. DiagnosticFix

```typescript
interface DiagnosticFix {

    title: string;

    edits: TextEdit[];

}
```

---

# 17. BuildMetrics

```typescript
interface BuildMetrics {

    buildTime: number;

    resources: number;

    cacheHit: number;

}
```

---

# 18. ValidationMetrics

```typescript
interface ValidationMetrics {

    validationTime: number;

    rulesExecuted: number;

    diagnostics: number;

}
```

---

# 19. GeneratorMetrics

```typescript
interface GeneratorMetrics {

    generationTime: number;

    filesGenerated: number;

    packageSize: number;

}
```

---

# 20. Event DTO

모든 Event는 다음 구조를 따른다.

```typescript
interface Event<T> {

    type: string;

    payload: T;

    timestamp: string;

}
```

---

# 21. Configuration DTO

```typescript
interface Configuration {

    builder: object;

    validator: object;

    generator: object;

}
```

---

# 22. Version DTO

```typescript
interface VersionInfo {

    builderVersion: Version;

    schemaVersion: Version;

    registryVersion: Version;

}
```

---

# 23. DTO Versioning

DTO는 Semantic Versioning을 따른다.

규칙

- PATCH : 설명 수정
- MINOR : Optional Property 추가
- MAJOR : Breaking Change

---

# 24. Compatibility Rules

Minor Version에서는 기존 DTO를 깨뜨리지 않는다.

Required Property 제거는 MAJOR 변경으로 간주한다.

---

# 25. Examples

Builder

```typescript
const result: BuildResult = await builder.build();
```

Validator

```typescript
const result = await validator.validate(context);
```

Generator

```typescript
const result = await generator.generate(context);
```

---

# Revision History

| Version | Description |
|----------|-------------|
| 1.0 | Initial DTO Specification |