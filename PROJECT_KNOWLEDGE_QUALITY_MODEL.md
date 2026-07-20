# Knowledge Quality Model

## 1. Purpose

This document defines the quality model used to evaluate all project knowledge within the VSkript Data Platform.

Its purpose is to establish a consistent and objective foundation for reviewing project artifacts, ensuring that every document contributes to a single, coherent Source of Truth.

This quality model applies to all knowledge artifacts, including but not limited to:

- Project documents
- Specifications
- RFCs
- Reference documents
- Architecture documents
- Review reports

This document does not define review procedures or implementation details.

Those are defined by the Review Framework and associated review processes.

---

# 2. Scope

This model defines the quality characteristics that every project artifact shall be evaluated against.

The model provides a common vocabulary for quality assessment and serves as the foundation for all review activities.

---

# 3. Quality Characteristics

Every knowledge artifact shall be evaluated using the following quality characteristics.

## 3.1 Correctness

Correctness determines whether the knowledge represented by an artifact is accurate, verifiable, and supported by evidence.

Questions include:

- Is the information factually correct?
- Is every important statement supported by evidence?
- Are assumptions clearly distinguished from facts?

---

## 3.2 Ownership

Ownership determines whether each concept has a single authoritative owner.

Questions include:

- Is the owner of each concept clearly identified?
- Is the concept defined only once?
- Does the document avoid redefining concepts owned elsewhere?

---

## 3.3 Consistency

Consistency determines whether the artifact aligns with the rest of the documentation system.

Questions include:

- Are terms used consistently?
- Does the document conflict with other documents?
- Are references coherent and traceable?

---

## 3.4 Completeness

Completeness determines whether the artifact contains all information required to fulfill its responsibility.

Questions include:

- Are required sections present?
- Are important concepts missing?
- Does the document fully satisfy its intended purpose?

---

## 3.5 Maintainability

Maintainability determines whether the artifact can evolve without introducing unnecessary complexity.

Questions include:

- Can the document be modified without widespread duplication?
- Is the structure scalable?
- Are responsibilities clearly separated?

---

## 3.6 Traceability

Traceability determines whether every important concept can be traced to its origin and related artifacts.

Questions include:

- Can this knowledge be traced to project goals or requirements?
- Are related documents properly referenced?
- Is the rationale discoverable?

---

# 4. Quality Evaluation

Every review performed within the project shall evaluate the applicable quality characteristics defined in this model.

Not every artifact is required to satisfy every characteristic equally.

The applicable review criteria depend on the artifact type.

---

# 5. Relationship to Review Framework

This model defines **what quality means**.

The Review Framework defines **how quality is evaluated**.

Review Prompts define **how reviews are executed**.

---

# 6. Relationship to Other Documents

This document is the foundation of the project's quality assurance system.

It supports:

- Review Framework
- Review Workflow
- Review Checklists
- Review Prompts
- Review Reports

It does not replace any of these documents.