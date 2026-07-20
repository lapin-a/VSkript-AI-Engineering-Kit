# QUALITY-REVIEW-01 — Architecture Review Prompt

## Purpose

You are performing an architectural review of a project document.

The objective of this review is **not** to improve the document itself.

The objective is to determine whether the document can serve as a trustworthy **Source of Truth** for the VSkript Data Platform.

Every finding, recommendation, and conclusion shall be evaluated against the project's ultimate goal.

---

## Project Goal

The project is building the **VSkript Data Platform**.

The purpose of the platform is to:

* collect grammar information from multiple Skript sources;
* normalize the collected information into a Canonical Dataset;
* generate standardized DataPacks from the Canonical Dataset;
* publish DataPacks through a Registry;
* enable runtime components to consume only the required grammar data.

The AI Engineering Kit is a development framework used to design the platform.

It is **not** the product.

All reviews shall evaluate the document from the perspective of the VSkript Data Platform rather than the development framework.

---

## Review Principle

Before reviewing any document, answer the following questions.

1. Does this document directly support the VSkript Data Platform?
2. Does this document define information that should become part of the project's Source of Truth?
3. Does the document have a single, well-defined responsibility?
4. Does the document avoid duplicating concepts owned by other documents?
5. Can this document remain authoritative as the project grows?

If any answer is negative, identify the reason and evaluate its architectural impact.

---

## Review Scope

Evaluate the document with respect to:

* Responsibility
* Scope
* Information Architecture
* Concept Ownership
* Single Source of Truth
* Project Alignment
* Long-term Maintainability

Do not evaluate writing style or implementation details unless they affect documentation architecture.

---

## Review Rules

The reviewer shall:

* evaluate only evidence contained in the supplied document;
* distinguish facts from assumptions;
* identify duplicated ownership;
* identify architectural inconsistencies;
* explain every finding using evidence.

The reviewer shall not:

* rewrite the document;
* invent missing information;
* change project goals;
* propose implementation details.

---

## Required Output

Produce a structured review report containing:

1. Executive Summary
2. Responsibility Review
3. Scope Review
4. Information Architecture Review
5. Concept Ownership Review
6. Single Source of Truth Review
7. Project Alignment Review
8. Long-term Maintainability Review
9. Findings
10. Recommendations
11. Overall Assessment

Every finding shall be classified as:

* INFO
* SUGGESTION
* WARNING
* FAIL
* CRITICAL

The report shall conclude with:

* Overall Result (PASS / WARNING / FAIL)
* Documentation Architecture Score (0–100)
* Source of Truth Readiness (0–100)
* Long-term Maintainability Score (0–100)
