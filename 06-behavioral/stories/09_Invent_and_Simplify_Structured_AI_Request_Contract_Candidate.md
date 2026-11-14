# Story 9 — Invent and Simplify: Candidate — Structured AI Request Contract

**Status:** Candidate selected; blocked on one factual confirmation.  
**Primary LP:** Invent and Simplify  
**Professional source:** Birlasoft AI-enabled commerce work only.

> Integrity boundary: The resume confirms that I implemented **structured outputs, validation schemas, idempotency controls and prompt guardrails**, with a reported **42% reduction in invalid and duplicate AI-generated order requests before transaction processing**. It does **not** currently establish the exact before-state or prove that these controls replaced ad-hoc parsing/validation branches. Do not present that simplification sequence as history until confirmed.

## Why this is the strongest Story 9 candidate

This work is distinct from:
- Story 3 Ownership, which is about specification-first delivery of inventory and order-status MCP tools;
- Story 2 Customer Obsession, which is about one QA-discovered invalid-order handling incident;
- Story 12 Broad Responsibility, which will focus on OAuth2/JWT/RBAC/scoped tool permissions.

The Invent and Simplify angle would be the **request-contract mechanism** around AI-triggered commerce actions: make model output predictable enough for deterministic backend processing rather than spreading AI-specific parsing and validation logic across multiple paths.

## Grounded facts

- Java 21 / Spring Boot AI services
- Spring AI / MCP / tool calling
- structured outputs
- validation schemas
- idempotency controls
- prompt guardrails
- reported **42% reduction in invalid and duplicate AI-generated order requests before transaction processing**

## Candidate STAR core — only after confirmation

**Potential Situation:** AI-generated tool requests needed predictable handling before transaction processing.

**Potential simplification:** normalize model output into a structured request contract with schema validation and idempotency controls, instead of allowing multiple loosely interpreted request shapes.

**Potential trade-off:** stricter contracts reduce model flexibility and require schema/version maintenance, but they simplify downstream processing and make failures deterministic.

**Potential result:** broader work reports **42% fewer invalid and duplicate AI-generated order requests before transaction processing**.

## One factual confirmation required

Did this work actually replace or reduce **multiple ad-hoc/unstructured parsing or validation paths** with a more consistent structured request/schema approach?

- If **yes**, this becomes a strong Invent and Simplify story.
- If **no**, do not use this mechanism as the story; recover a different professional simplification incident.

## Do not claim yet

- that there were N duplicated parsers/branches;
- that the team previously accepted free-form LLM text directly into transactions;
- that I personally designed every validation/idempotency mechanism;
- a specific schema technology or annotation unless confirmed;
- that the 42% came only from structured outputs rather than the broader set of controls.
