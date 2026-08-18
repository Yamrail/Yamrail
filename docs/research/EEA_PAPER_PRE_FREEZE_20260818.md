# EEA Paper — Pre-Freeze Research Card

**Status:** PRE-FREEZE

**Snapshot date:** 2026-08-18
**Repository:** https://github.com/Yamrail/Yamrail.git
**Baseline:** main at 3a0d0163759f0e484d2fb4149ebaa2139bc10224

## Scope

This card records an evidence-based review of the public YamRail repository at the
baseline above. It describes the repository artifacts, their declared relationships,
and the engineering limits that can be established from the snapshot. It is not a
runtime safety certification and does not claim behavior that is not represented by
the tracked files.

## Method and provenance

The review used the public clone and the following read-only checks:

    git ls-tree -r --name-only HEAD
    git status --porcelain
    git config --get remote.origin.url
    git branch --show-current
    git rev-parse HEAD

The resulting provenance is:

| Check | Result |
|---|---|
| Remote | https://github.com/Yamrail/Yamrail.git |
| Branch | main |
| HEAD | 3a0d0163759f0e484d2fb4149ebaa2139bc10224 |
| Working tree | clean at review start |

## Evidence inventory

| Artifact | Evidence captured |
|---|---|
| packages/core-engine/src/prompts/yamrail_core_v0.7.0_lite.yaml | Core Lite v0.7.0; declares CoreSafety.v1, Syntax, CalibrationLayer, OutputProtocol, and a one-way full_spec_ref to Core Full v0.6.7. |
| packages/core-engine/src/prompts/yamrail_core_v0.6.7_full.yaml | Core Full v0.6.7; defines the profile state machine, four-axis temperature score, paradox engine, safety valves, and standard output fields. |
| docs/yamrail_guidebook_v0.7.1.txt | Human-facing design guide and terminology context. |
| docs/yamrail_appendix_b_v0.7.1.txt | Cross-reference rules between the guidebook and Core artifacts. |
| docs/appendices/appendix_043_revised_specification_full.yaml | Present in the baseline tree as a line-only placeholder; no executable behavior can be inferred from it. |
| README.md | Public purpose, two-tier Core model, usage sequence, and declared score ranges. |

## Findings

### 1. Artifact identity and dependency direction

YamRail is published as a declarative prompt/specification package. Core Lite is the
small, repeatedly loaded surface; Core Full is the detailed reference. The Lite file
explicitly delegates detailed definitions to v0.6.7 and states that the dependency is
one-way. No runtime entry point, package manifest, test suite, or service configuration
is present in the tracked baseline tree.

### 2. Declared safety boundary

CoreSafety.v1 separates concrete harmful-how requests from conceptual paradoxes.
The former are assigned force_terminate with reason disclosure; the latter remain
eligible for normal ParadoxRail handling. The declared invariants keep final
responsibility with the human Observer and prohibit external sending or execution by
the AI. These are specification-level controls, not a demonstrated enforcement
mechanism in a running program.

### 3. Calibration, scoring, and output contract

Calibration is declared manual (auto: false) with explicit triggers and a
transparency requirement. The Full spec models temperature as a weighted sum over
coherence_deviation, boundary_pressure, paradox_density, and
self_reference_depth, normalized to 0.0–2.0. The standard output contract names
current temperature, active profile, fermentation stage, paradox summary, risk levels,
main output, and meta-commentary as fields.

### 4. Paradox handling and human gate

The Full spec classifies recursive self-reference, observer effect, role inversion, and
infinite regress. Its safety limiter sets max_weaponization_temperature to 1.85
and requires human approval for the listed high-pressure conditions. The repository
does not include an executable evaluator that proves those predicates are enforced.

## Pre-freeze engineering assessment

| Area | Assessment | Freeze implication |
|---|---|---|
| Provenance | Reproducible from the public remote, branch, and commit above. | Accept for this snapshot. |
| Specification layering | Lite-to-Full references are explicit and inspectable. | Preserve the direction of dependency. |
| Safety semantics | Boundaries and escalation conditions are documented in YAML. | Treat as declarative policy until an evaluator exists. |
| Runtime verification | No runtime/test harness is present in the tracked tree. | Do not claim implementation-level enforcement. |
| Documentation completeness | The appendix placeholder has no substantive payload. | Keep it out of behavioral claims; review before a documentation freeze. |

## Pre-freeze decision

**CONDITIONAL ACCEPTANCE for a public research/documentation snapshot.** The
repository identity, artifact relationships, and declared boundaries are sufficiently
traceable for the research card. A full engineering release would still require a
machine-readable validation path and tests that exercise the declared safety and
output contracts.

## Acceptance record

- Scope limited to the public clone at the stated commit.
- No Core, package, historical document, or license text was changed.
- No commit or push was performed.
- This card and the companion engineering abstract are the only research additions.
