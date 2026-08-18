# Evidence- and Boundary-Constrained LLM Behavior

## A Reproducibility-First Engineering Study of YamRail / Evaluation Environment Audit (EEA)

**Author:** 大津岳広
**Status:** MAIN MEASUREMENT COMPLETE / PRE-FREEZE FINALIZATION
**Target public repo:** `Yamrail/Yamrail`
**Staging status:** `PUBLIC_RELEASE_CANDIDATE`

> This is a public pre-freeze paper card candidate. Final completion inspection is still pending; this is not the final frozen manuscript.

---

## Abstract

Large language models can continue a task even when evidence, authority, or state information is incomplete. In practical work, however, the ability to continue is not always equivalent to permission to continue, and plausible completion is not always equivalent to evidence-backed completion.

This study examines a set of operational constraints that emerged during the development of YamRail, an evidence- and boundary-oriented workflow for human–LLM collaboration. Rather than beginning from an abstract safety theory, the study first identifies engineering practices that were actually used during development: retaining `UNKNOWN` and `HOLD` when required evidence was missing, separating capability from authority, validating artifact identity through parser/hash/attachment consistency, preserving prior failure states after later recovery, and maintaining traceable evidence references.

These practices were converted into reproducible interventions that can be applied independently of YamRail itself. Five hypotheses were evaluated through paired baseline/constrained fixtures: unsupported-PASS suppression, non-expansive authority retention, state-history preservation, evidence reachability, and bounded utility. The completed main measurement comprised 30 experimental units (N=3 per condition per fixture) and 36 successful provider requests in a single model series. H1 and H2 were not demonstrated because the Baseline condition already satisfied the target behavior in ceiling-observed controls. H3 and H4 showed complete observed separation within their tested fixtures. H5 satisfied the defined bounded-utility behavior but showed no incremental difference from Baseline. These are fixture-level observations from a single model series; no statistical-significance or cross-model/provider generalization claim is made.

The primary research question is not whether a model becomes “safe” in the abstract, but whether observable behavior changes when evidence and authority boundaries are made explicit. The study also examines whether stronger boundary preservation merely increases refusal, or whether an execution actor can remain useful inside authorized and evidenced scope while stopping only at unsupported or unauthorized boundaries.

**Keywords:** LLM, evidence, authority boundary, reproducibility, human–AI collaboration, provenance, fail-closed, non-expansive authority

---

## Main engineering question

> Can an LLM remain operationally useful while preserving explicit boundaries between evidence, authority, state history, and human judgment?

Independent evaluation axes:

- Evidence
- Authority
- State history
- Evidence reachability
- Task result / bounded utility

Core authority rule:

> **Capability != Permission**

An execution actor may act within explicitly granted scope, but may not enlarge its own authority, infer missing authority, or convert technical capability into permission.

---

## Main measurement

`5 fixtures × 2 conditions × 3 repetitions = 30 experimental units`

The requested model was `gpt-5.2`; the returned observable snapshot was `gpt-5.2-2025-12-11` throughout the completed series. Provider-internal routing and serving conditions were not observable and are not claimed to have been fixed.

| Hypothesis | Result |
|---|---|
| H1 Unsupported-PASS Suppression | NOT_DEMONSTRATED — ceiling observed |
| H2 Non-expansive Authority Retention | NOT_DEMONSTRATED — ceiling observed |
| H3 State-History Preservation | SUPPORTED_IN_THIS_FIXTURE |
| H4 Evidence Reachability | SUPPORTED_IN_THIS_FIXTURE |
| H5 Bounded Utility | SUPPORTED_AS_DEFINED — no incremental difference from Baseline |

Observed separations:

- H3 prior failure retained: Baseline `0/3`, Constrained `3/3`
- H4 full judgment-to-evidence reachability: Baseline `0/3`, Constrained `3/3`

Ceiling effects and non-incremental results are retained rather than rewritten around convenient outcomes.

---

## Claim boundary

This study does **not** claim:

- that YamRail solves general AI safety;
- that the results generalize across models or providers;
- statistical significance from `N=3`;
- validation of an internal causal mechanism;
- that a matching commercial model label proves identical provider-side execution conditions;
- that boundary preservation is equivalent to technical correctness.

The supported claim is narrower: explicit evidence, authority, state-history, and reachability constraints can be turned into reproducible behavioral test fixtures, and observable differences appeared on some axes in the tested fixture set.

---

## Reproducibility position

Minimum reproduction is intended to require only:

1. an LLM;
2. fresh independent sessions;
3. fixed fixture text;
4. fixed intervention text;
5. a visible evaluation rubric.

For closed frontier APIs, the study distinguishes historical observation, ecological replication, and a future fixed-reference reproduction environment. A hosted model name or snapshot identifier is not treated as proof of bit-identical execution.

---

## Publication status

The source manuscript records the main measurement as complete and inspected, with the citation and reproducibility layers closed. Final completion inspection and final freeze are still pending. The eventual frozen manuscript and reproduction package should supersede this page without deleting the pre-freeze history.
