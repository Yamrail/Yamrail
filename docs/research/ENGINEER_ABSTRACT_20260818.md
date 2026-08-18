# YamRail — Engineering Abstract

**Snapshot:** main @ 3a0d0163759f0e484d2fb4149ebaa2139bc10224
**Remote:** https://github.com/Yamrail/Yamrail.git

## Abstract

YamRail is a declarative intermediate layer for structuring collaboration between a
human Observer and language models. The public snapshot is specification-first: it
contains versioned YAML prompt contracts and explanatory documents, but no runtime,
package manifest, test harness, or external-action adapter. Its principal engineering
design is a two-tier Core: Core Lite v0.7.0 is the repeatedly loaded contract, while
Core Full v0.6.7 supplies the detailed reference through explicit one-way links.

## Architecture in one view

    human prompt
        -> Core Lite v0.7.0
           -> Syntax / InferenceRail
           -> CalibrationLayer (manual, transparent)
           -> OutputProtocol
           -> Core Full v0.6.7 for detailed scoring, paradox, and safety definitions

The public README and guidebook provide the human-facing vocabulary. Appendix B
defines cross-reference rules; the Core files remain the authoritative implementation
contracts for the declared behavior.

## Safety and control semantics

- CoreSafety.v1 distinguishes concrete harmful-how requests from conceptual paradox.
- Concrete harmful-how requests are assigned termination plus reason disclosure.
- Conceptual paradox is routed through ParadoxRail unless the absolute boundary is met.
- The Observer retains final responsibility; the specification disallows external
  sending or execution by the AI.
- Calibration is not automatic and must be transparent when invoked.
- The standard output contract exposes state and risk fields alongside the main answer.

These are policy and prompt semantics. They should not be presented as runtime
guarantees until an evaluator and tests are added.

## Engineering readiness

The snapshot is suitable for documentation and research review. It is not yet a
standalone executable package: there is no declared build entry point, dependency
manifest, or automated test path in the tracked tree. The next engineering increment
should validate YAML structure, resolve all referenced files, and exercise the safety
and output contracts with deterministic fixtures.

For the evidence table and pre-freeze decision, see
[EEA_PAPER_PRE_FREEZE_20260818.md](EEA_PAPER_PRE_FREEZE_20260818.md).
