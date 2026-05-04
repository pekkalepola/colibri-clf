# COLIBRI — Concept Library File (CLF)
Single Concept Format Specification v0.9

This repository publishes the CLF 0.9 pre-standard draft —
the file format for a single concept in the COLIBRI Concept Library.

## Concept Purity Rule

A concept file expresses only what a concept IS and what is perceptually typical of it.
It contains no detection logic, no rules, no algorithms, no thresholds, and no references to other concepts.
All interpretation belongs exclusively to the control layer.

A concept file MUST NOT contain any structure, field, reference, or wording that can be interpreted as prescribing, constraining, guiding, or implying any behavior of the control layer, inference layer, or any system that consumes the concept.

## Format Status

This specification is published at version 0.9. The format is intentionally not yet final.

The goal of the COLIBRI Concept Library is broad adoption across industries and systems. A format that is to become a shared standard must be shaped by those who will use it.

## Status of This Document

This document specifies the CLF 0.9 pre-standard draft — the file format for a single concept in the COLIBRI Concept Library. CLF is one component of a broader standard currently under development, which will also cover registration practices, concept identifier naming, versioning at the library level, and governance.

This is not the final standard. The final specification will be decided together with early adopters, implementers, and domain experts.

No rights to implement, deploy, or integrate this format are granted.
All commercial and technical usage requires a separate license from the rights holder.

Underlying invention protected by Finnish Utility Model No. 13913.

## Contents

| File | Description |
|------|-------------|
| `clf_concept_spec_v1.md` | Single concept format specification v0.9 |
| `Concept_library_wp.pdf` | White paper — full theoretical foundation |
| `C-002.electric_arc/` | Example concept — electric arc (phenomenon) |
| `C-003.partial_discharge/` | Example concept — partial discharge (phenomenon) |
| `C-004.abnormal_vibration/` | Example concept — abnormal vibration (phenomenon) |

## Example Concepts

Three example concepts are included to demonstrate the CLF format across different industrial domains.
Each concept is expressed as a folder structure representing the contents of a `.clf` archive.

The examples cover phenomena relevant to electrical insulation, high-voltage equipment, and rotating machinery —
domains where precise, system-independent concept definitions have direct practical value.

## White Paper

*Concept Library*
by Pekka Lepola, 2026.

The white paper presents the theoretical foundation, architectural consequences,
EU AI Act implications, and a proof-of-concept semantic decision trace.

---

© 2026 Pekka Lepola. All rights reserved.

The concept library and control layer invention is protected by
Finnish Utility Model No. 13913.

No part of this specification may be used, reproduced, or implemented
without explicit written permission from the rights holder.

conceptlibrary.eu@gmail.com
