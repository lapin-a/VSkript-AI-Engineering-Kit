# GLOSSARY

> **Status:** Informative  
> **Version:** 1.0.0  
> **Category:** Reference  
> **Scope:** VSkript Data Platform  
> **Normative Definitions:** See the owning Specification of each term.

---

# Introduction

This glossary provides a common vocabulary for the VSkript Data Platform.

Its purpose is to establish a shared language across the project and improve consistency between Specifications, RFCs, Architecture documents, implementation, and AI-generated artifacts.

This document is **informative**.

Normative definitions are owned by their respective Specifications.

Where a term is defined by a Specification, this glossary provides only a concise description and references the owning document.

---

# Reading Guide

Each glossary entry contains:

- **Term** — The preferred project term.
- **Summary** — A concise description.
- **Owner** — The Specification that normatively defines the concept.

---

# Terms

---

## Addon

A Skript extension that contributes additional grammar elements to the Skript ecosystem.

**Owner**

TBD

---

## Artifact

A generated deliverable produced from platform data.

Examples include DataPacks, indexes, metadata files, and manifests.

**Owner**

TBD

---

## Builder

A platform component responsible for transforming canonical data into distributable artifacts.

The Builder does not define grammar.

It generates artifacts from existing specifications and canonical data.

**Owner**

TBD

---

## Canonical Dataset

The authoritative, implementation-independent representation of grammar data managed by the platform.

The Canonical Dataset serves as the single source from which distributable artifacts are generated.

**Owner**

SPEC-0002

---

## Consumer

Any software component that consumes artifacts produced by the platform.

Examples include:

- VSCode Extension
- Language Server
- CLI
- Future runtime integrations

**Owner**

TBD

---

## DataPack

A distributable grammar artifact generated from the Canonical Dataset.

A DataPack contains grammar information intended for consumption by runtime components.

**Owner**

SPEC-0004

---

## Distribution

The process of making platform artifacts available to consumers.

Distribution is independent of artifact generation.

**Owner**

TBD

---

## Grammar

A structured representation of Skript language constructs.

Grammar includes expressions, effects, events, conditions, functions, types, and related language elements.

**Owner**

SPEC-0003

---

## Grammar Source

An external source from which grammar information is acquired.

Examples include:

- SkriptHub
- Addon documentation
- Official repositories
- Verified community resources

**Owner**

TBD

---

## Metadata

Descriptive information about platform artifacts.

Metadata is not grammar itself.

It describes grammar artifacts.

**Owner**

SPEC-0006

---

## Platform

The complete VSkript Data Platform responsible for collecting, normalizing, managing, and distributing grammar data.

**Owner**

SPEC-0001

---

## Provider

A platform component that supplies data to another component.

The specific responsibilities depend on the context in which the term is used.

**Owner**

TBD

---

## Registry

A platform service responsible for publishing, indexing, and resolving distributable artifacts.

**Owner**

SPEC-0005

---

## Runtime

An execution environment capable of consuming platform artifacts.

Examples include editors, language servers, command-line tools, and future integrations.

**Owner**

TBD

---

## Specification

A normative document that defines required concepts, contracts, or behaviors within the project.

Specifications are the primary Source of Truth for platform definitions.

**Owner**

SPEC-0001

---

## Ubiquitous Language

The common vocabulary shared across the project.

Every project document should use these terms consistently.

**Owner**

SPEC-0001

---

# Preferred Terminology

The following preferred terms should be used consistently throughout the project.

| Preferred | Avoid |
|-----------|-------|
| Canonical Dataset | Internal Data |
| DataPack | Package |
| Grammar Source | Source File |
| Runtime | Client |
| Consumer | User Program |
| Registry | Package Server |
| Artifact | Output File |

---

# Related Documents

- PROJECT.md
- REQUIREMENTS.md
- ARCHITECTURE.md
- SPEC-0001