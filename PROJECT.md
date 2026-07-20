# PROJECT.md

# VSkript Data Platform

Version: 2.1 (Draft)

---

# 1. Vision

Build the standard Grammar Data Platform for the global Skript ecosystem.

The platform enables every Skript Runtime to consume a common set of grammar data through standardized DataPacks generated from a shared Canonical Dataset.

Each Runtime shall download only the DataPacks required for its current workspace, enabling a scalable, modular, and runtime-independent grammar ecosystem.

### Concept Flow
```
Grammar Sources
↓
Collection
↓
Canonical Dataset
↓
Specification
↓
DataPack
↓
Registry
↓
Runtime Consumers
```


The Canonical Dataset is the core asset of the platform.

Every distributable artifact is derived from this dataset.

---

# 2. Mission

The mission of the VSkript Data Platform is to collect grammar information from SkriptHub and various Skript Addons, normalize the collected information into a Canonical Dataset, and distribute standardized DataPacks through a Registry.

The platform exists to establish a reusable, maintainable, and implementation-independent grammar distribution ecosystem.

The AI Engineering Kit is a development framework used to design and evolve the platform.

It is not the product itself.

---

# 3. Background

The current Skript ecosystem suffers from several structural limitations.

- Grammar information is distributed across multiple repositories.
- Each Addon defines its grammar using different formats.
- Tracking changes across Addons is difficult.
- Runtime implementations must bundle large amounts of grammar data.
- Localization support is inconsistent.
- There is no canonical representation of grammar shared across runtimes.

These limitations make long-term maintenance increasingly difficult.

The VSkript Data Platform addresses these issues through standardization, normalization, and centralized distribution.

---

# 4. Problem Statement

The existing ecosystem lacks a standardized grammar distribution platform.

As a result:

- Runtime components cannot selectively download only the required grammar.
- Incremental grammar updates are difficult.
- There is no canonical model representing Skript grammar.
- Grammar knowledge is duplicated across multiple runtimes.
- Each runtime must solve the same grammar management problems independently.

---

# 5. Goals

The platform aims to achieve the following objectives.

- Collect grammar information from SkriptHub and community Addons.
- Normalize collected grammar into a Canonical Dataset.
- Maintain the Canonical Dataset as the authoritative grammar source.
- Generate standardized DataPacks from the Canonical Dataset.
- Publish DataPacks through a Registry.
- Enable Runtime components to download only the required DataPacks.
- Support incremental updates by distributing only changed data.
- Provide a multilingual grammar platform reusable by multiple runtimes.

---

# 6. Non Goals

The project does not aim to build or replace the following systems.

- Skript Runtime
- Skript Compiler
- Plugin Loader
- SkriptHub Service
- IDE Implementation
- Language Runtime

The platform focuses exclusively on grammar data management and distribution.

---

# 7. Target Users

The platform is intended for:

- VSkript users
- VSCode Extension developers
- Language Server developers
- CLI developers
- Static analysis tools
- Documentation generators
- AI-assisted development tools
- Future runtime implementations

---

# 8. Scope

The project includes:

- Grammar Collection
- Data Normalization
- Canonical Dataset
- Grammar Specification
- DataPack Generation
- Registry
- Distribution
- Runtime Loading
- Localization
- Version Management
- Incremental Synchronization

---

# 9. Success Criteria

The project is considered successful when:

- Every grammar definition exists within the Canonical Dataset.
- Runtime components download only the required DataPacks.
- Grammar updates distribute only changed information.
- Multiple runtimes reuse identical DataPacks.
- Registry becomes the single distribution entry point.
- Grammar data remains implementation-independent.
- Canonical Dataset serves as the authoritative grammar source.

---

# 10. Core Principles

The project follows the following principles.

- Specification First
- Canonical First
- Source of Truth
- Evidence First
- Project First
- Runtime Independent
- Data-driven Design
- Automation Friendly
- Modular Architecture
- Reproducible

---

# 11. Source of Truth

The VSkript Data Platform establishes explicit Sources of Truth to ensure consistency, traceability, and long-term maintainability.

The ownership of knowledge is defined as follows.

| Knowledge | Source of Truth |
|-----------|-----------------|
| Project Vision | PROJECT.md |
| Project Requirements | REQUIREMENTS.md |
| System Architecture | ARCHITECTURE.md |
| Technical Specifications | SPEC Documents |
| Design Decisions | RFC Documents |
| Terminology | REFERENCE/GLOSSARY.md |
| Grammar Data | Canonical Dataset |

No document shall redefine concepts owned by another Source of Truth.

Every project artifact shall either own a concept or reference its authoritative owner.

---

# 12. Expected Outcomes

When the platform is complete, the following capabilities shall be available.

- Opening a project automatically identifies required Skript Addons.
- Only the required Grammar DataPacks are downloaded.
- Grammar updates are distributed incrementally.
- Multiple runtimes reuse identical grammar packages.
- Grammar knowledge remains consistent across every runtime.
- Users worldwide share the same standardized grammar platform regardless of implementation.

---

# 13. Long-term Roadmap

The platform is designed to evolve over time.

Long-term objectives include:

- Remote Registry
- Distributed CDN
- Incremental Synchronization
- Automated Grammar Validation
- Multi-language Grammar Database
- Marketplace Integration
- Third-party Runtime Support
- Community DataPack Publishing
- Canonical Dataset Version History

These roadmap items describe future directions and shall not be interpreted as implementation commitments.

---

# 14. Project Philosophy

The VSkript Data Platform is fundamentally a data platform rather than a runtime project.

Its primary asset is the Canonical Dataset.

Every architecture decision, specification, and implementation shall support the lifecycle of grammar knowledge rather than the lifecycle of individual runtime implementations.

Specifications define the platform.

Implementations realize the specifications.

Runtime components consume the artifacts produced by the platform.

The platform therefore remains implementation-independent while providing a common foundation for every consumer.

---

# 15. Document Relationship

This document defines the purpose and direction of the project.

The remaining documents refine this vision.

```

PROJECT.md
│
├── REQUIREMENTS.md
│
├── ARCHITECTURE.md
│
├── SPEC/
│
├── RFC/
│
├── REFERENCE/
│
└── QUALITY/

```


This document does not define implementation details.

Implementation decisions belong to Architecture, Specifications, and RFCs.

---

# 16. Document Information

| Item | Value |
|------|-------|
| Document | PROJECT.md |
| Version | 2.1 |
| Status | Draft |
| Owner | VSkript Data Platform |
| Classification | Project Definition |
| Source of Truth | Project Vision and Direction |

---

# 17. Summary

The VSkript Data Platform standardizes grammar knowledge across the Skript ecosystem.

Its primary objective is to collect grammar information from multiple sources, normalize that information into a Canonical Dataset, and distribute reusable DataPacks through a Registry.

The Canonical Dataset is the platform's core asset and the authoritative source of grammar knowledge.

Every Specification, Runtime, Registry, and DataPack ultimately derives from this shared foundation, ensuring a consistent, reusable, and implementation-independent grammar ecosystem.