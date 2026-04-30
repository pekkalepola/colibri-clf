# COLIBRI Concept Library File (CLF) — Full Specification v0.9
## The Canonical Format for an Atomic Semantic Concept

---

## Preface

A concept is the smallest indivisible unit of shared meaning.
It represents exactly one thing — and it represents that thing completely,
across all modalities through which it can be perceived or recognized.

This specification defines the format of a single concept in the COLIBRI Concept Library.
It is designed to be:

- **Universal** — applicable to physical states, abstract ideas, mathematical objects,
  biological phenomena, sensory experiences, and any other category of meaning
- **Multimodal** — captures how a concept is expressed in language, sound, image,
  signal, touch, chemistry, and electromagnetic spectrum
- **Immutable** — a published concept version never changes
- **Verifiable** — every concept container is cryptographically sealed
- **Intelligence-free** — a concept defines only meaning, never inference, thresholds,
  causality, or algorithms

---

## 1. Core philosophy — what a concept IS and IS NOT

### What the concept defines:

- **What it is** — in natural language and formal terms
- **What it looks, sounds, and feels like** — perceptual signatures
- **How it is classified** — kind, domain, scope

### What the concept does NOT define:

- Detection thresholds (belongs to the control layer)
- Causal relationships (belongs to inference engines)
- Taxonomic hierarchies between concepts (belongs to ontology layers)
- Actions to take upon occurrence (belongs to rule engines)
- Algorithms to compute it (belongs to processing pipelines)


---

## 2. Container structure

A concept file is a single **ZIP archive** with the `.clf` extension.
It contains a defined directory structure. The file is atomic — one concept, one file, one hash.

```
<concept_id>.clf   ← ZIP archive
│
├── manifest.json                    [REQUIRED] — identity, integrity, index
├── taxonomy.json                    [REQUIRED] — classification and scope
│
├── payload/
│   │
│   ├── definition/
│   │   ├── canonical.txt            [REQUIRED] — human-readable definition
│   │   └── i18n/                   [optional] — translations
│   │       ├── fi.txt
│   │       ├── de.txt
│   │       ├── zh.txt
│   │       └── ...
│   │
│   ├── perceptual/                  [optional, per-modality]
│   │   ├── visual.json              — color, texture, shape, spectral characteristics
│   │   ├── acoustic.json            — frequency, temporal pattern, amplitude envelope
│   │   ├── signal.json              — time-series morphology (current, pressure, etc.)
│   │   ├── haptic.json              — surface temperature, texture, vibration
│   │   ├── chemical.json            — molecular, spectral, olfactory signatures
│   │   ├── electromagnetic.json     — EM spectral region, emission, polarization
│   │   └── extensions/              — future modalities (custom JSON)
│   │       └── <modality_name>.json
│   │
│   └── schema/
│       └── constraints.json         [optional] — necessary/sufficient conditions
│
└── signatures/
    └── signature.json               — cryptographic signature metadata
```

---

## 3. `manifest.json` — identity and integrity

```json
{
  "clf_version":         "0.9",
  "concept_id":          "C-001.overheat",
  "version":             "1.0.0",
  "issued_at":           "2026-04-28T10:15:00Z",
  "issuer_id":           "COLIBRI_DEMO",

  "concept_kind":        "state",
  "maturity":            "draft",

  "summary":             "A localized, temporary or sustained increase in temperature within a defined system component, exceeding its normal operating condition.",

  "modalities_present": ["linguistic", "perceptual.signal", "perceptual.visual"],

  "payload_hash_tree": {
    "taxonomy.json":                            "sha256-<hash>",
    "payload/definition/canonical.txt":         "sha256-<hash>",
    "payload/perceptual/visual.json":           "sha256-<hash>",
    "payload/perceptual/signal.json":           "sha256-<hash>",
    "payload/schema/constraints.json":          "sha256-<hash>"
  },

  "container_hash":      "sha256-<merkle root of all payload hashes>"
}
```

### Field definitions

| Field | Type | Required | Description |
|---|---|---|---|
| `clf_version` | string | YES | Format version of this specification |
| `concept_id` | string | YES | Globally unique. Format: `<registry>-<seq>.<name>` |
| `version` | string | YES | Semantic version of this concept (MAJOR.MINOR.PATCH) |
| `issued_at` | string | YES | ISO 8601 UTC timestamp |
| `issuer_id` | string | YES | Registry or organization identifier |
| `concept_kind` | string | YES | See section 4 |
| `maturity` | string | YES | `draft` / `proposed` / `stable` / `deprecated` |
| `summary` | string | YES | Short human-readable summary of the concept. Max 50 words. |
| `modalities_present` | array | YES | List of which optional payload sections exist |
| `payload_hash_tree` | object | YES | SHA-256 hash of every file in payload |
| `container_hash` | string | YES | Merkle root: SHA-256 of all payload hashes combined |

### Immutability rule

A `(concept_id, version)` pair is permanently immutable once published.
Corrections require a new PATCH version. Breaking changes require a new MAJOR version.
The old version is never deleted — it is marked `maturity: deprecated`.

### Absent field semantics

> **Absence of a field MUST be interpreted as:
> "This dimension is not part of the concept definition."**

A missing perceptual section (e.g. no `acoustic.json`) is a positive statement:
acoustic perception does not apply to this concept.
It is not an omission — it is part of the definition.

There is no "unspecified" or "unknown" state for a published concept.
If a dimension is uncertain, the concept is not ready for publication.

---

## 4. `taxonomy.json` — classification

```json
{
  "concept_kind":     "state",
  "domain":           "thermodynamics",
  "subdomain":        "thermal_stress",
  "scope":            "physical_systems",
  "abstraction_level": "observable",
  "labels":           ["overheat", "thermal_overload", "heat_excess"],
  "language":         "en",
  "keywords":         ["temperature", "thermal", "fault", "safety"]
}
```

### `concept_kind` — full taxonomy

| Kind | Description | Example |
|---|---|---|
| `state` | A system is in a named discrete condition | overheat, overcurrent, stall |
| `phenomenon` | An observable deviation or emergent event | abnormal_vibration, cavitation |
| `entity` | A class of physical or logical thing | bearing, transistor, enzyme |
| `property` | A measurable or observable attribute | temperature, viscosity, frequency |
| `event` | A discrete occurrence in time | collision, threshold_crossing, power_cycle |
| `process` | A named sequence or mode of operation | combustion, annealing, authentication |
| `action` | A purposeful operation performed by an agent | inject, rotate, encrypt |
| `pattern` | A recurring structural or behavioral motif | oscillation, drift, periodicity |
| `relation` | A type of connection (defined as a concept itself) | contains, precedes, calibrates |
| `abstract` | A mathematical or logical object | integral, prime_number, entropy |
| `biological` | A living system concept | apoptosis, photosynthesis, infection |
| `cognitive` | A mental or experiential concept | attention, recognition, fatigue |

### `abstraction_level`

| Value | Meaning |
|---|---|
| `observable` | Directly perceivable by sensors or humans |
| `inferable` | Requires inference from observables (still a concept, not inference itself) |
| `theoretical` | Abstract construct that organizes understanding |
| `mathematical` | Purely formal / symbolic |

---

## 5. `payload/definition/canonical.txt` — linguistic definition

The human-readable definition. All modalities — linguistic and perceptual —
are equal expressions of the same concept. No single modality is the authoritative source.
`canonical.txt` provides the linguistic expression of the concept.

**Required structure:**

```
Concept: <concept_id>
Kind: <concept_kind>

Definition:
<A clear statement of what this concept represents.>
<Written in present tense, third person, declarative.>
<Do not reference specific systems, thresholds, or algorithms.>

This concept does NOT define:
- <list exclusions explicitly>
- <one exclusion per line>

Semantic scope:
<Optional: what domain or context this definition applies to.>
<If the concept is universal, omit this section.>
```

**Writing rules:**

1. One concept = one thing. Never compound.
2. Define by essence, not by effect.
   - CORRECT: "A system is in an OVERHEAT state (characterized by excessive thermal condition)."
   - WRONG: "A system is overheating, which may cause damage."
3. No thresholds in the definition.
4. No causal statements.
5. Maximum 200 words. If longer, the concept is not atomic — split it.

---

## 6. `payload/perceptual/` — multimodal signatures

Perceptual signatures describe how this concept is typically expressed across
different sensory and measurement modalities.

These are **characteristic patterns** — not detection thresholds.
They answer: "What does this concept tend to look/sound/feel/measure like?"

All modalities are equal in authority. No single modality defines the concept —
the concept is the complete set of all its expressions across modalities.

### Allowed encoding formats per modality

When a perceptual section includes binary data files (waveforms, images, signals),
the following formats are permitted:

| Modality | Allowed formats | Recommended |
|---|---|---|
| `acoustic` | Opus, AAC, FLAC | Opus |
| `visual` | WebP, AVIF, PNG | WebP |
| `signal` | JSON array, CSV, MessagePack | JSON array |
| `haptic` | JSON (numeric profile) | JSON |
| `chemical` | JSON, SMILES string | JSON |
| `electromagnetic` | JSON, CSV | JSON |
| `extensions` | Defined per extension | — |

Formats not listed above MUST NOT be used in published concepts.
Parsers may assume these formats are the only ones present.

---

### 7.1 `visual.json`

```json
{
  "modality": "visual",
  "applicability": "high",
  "applicability_note": "Overheat has strong visual character in the visible and thermal infrared spectrum.",

  "spectral_bands": ["visible_light", "near_infrared", "thermal_infrared"],
  "primary_spectral_band": "thermal_infrared",

  "color_profile": {
    "space": "HSV",
    "dominant_hues": ["red", "orange", "yellow"],
    "hue_range_deg": [0, 60],
    "saturation": "high",
    "value": "high",
    "notes": "Visible spectrum: surface discoloration. Thermal infrared: elevated radiance."
  },

  "texture_descriptor": {
    "uniformity": "non-uniform",
    "gradient_type": "radial",
    "gradient_direction": "center_to_edge"
  },

  "shape_descriptor": null,

  "motion_descriptor": {
    "motion_present": true,
    "motion_type": "convective"
  },

  "prototype_image_present": false,
  "prototype_image_format": null,
  "prototype_image_file": null
}
```

---

### 7.2 `acoustic.json`

```json
{
  "modality": "acoustic",
  "applicability": "medium",
  "applicability_note": "Acoustic character varies by system material and configuration.",

  "frequency_range_hz": {
    "low": 500,
    "high": 8000,
    "peak_region": "2000-5000"
  },

  "temporal_pattern": {
    "type": "intermittent",
    "duration_class": "milliseconds_to_seconds",
    "onset": "gradual"
  },

  "amplitude_envelope": {
    "attack": "slow",
    "decay": "slow",
    "sustain": "variable",
    "release": "slow"
  },

  "harmonic_structure": {
    "type": "broadband_noise",
    "fundamental_present": false
  },

  "prototype_waveform_present": false,
  "prototype_waveform_format": null,
  "prototype_waveform_file": null
}
```

---

### 7.3 `signal.json` — time-series and sensor signals

```json
{
  "modality": "signal",
  "applicability": "very_high",
  "applicability_note": "Overheat has primary signal character in the temperature domain and derived thermal quantities.",

  "signal_domains": [
    {
      "domain": "temperature",
      "unit": "K or degC",
      "morphology": "monotonic_rise_or_elevated_plateau",
      "temporal_scale": "seconds_to_hours",
      "characteristic_pattern": "sustained elevated temperature"
    },
    {
      "domain": "thermal_rate_of_change",
      "unit": "degC/min",
      "morphology": "positive_spike_or_positive_sustained",
      "temporal_scale": "seconds_to_minutes",
      "characteristic_pattern": "positive rate of temperature change"
    },
    {
      "domain": "infrared_radiance",
      "unit": "W/m2/sr",
      "morphology": "elevated_hotspot",
      "temporal_scale": "continuous",
      "characteristic_pattern": "localized region of elevated infrared radiance"
    }
  ],

  "stationarity": "non-stationary",
  "periodicity": "none"
}
```

---

### 7.4 `haptic.json` — tactile and thermoreceptive

```json
{
  "modality": "haptic",
  "applicability": "high",

  "surface_thermal": {
    "character": "hot_to_touch"
  },

  "surface_texture": {
    "change_present": true,
    "character": "possibly_rough"
  },

  "vibration_component": {
    "present": false
  }
}
```

---

### 7.5 `chemical.json` — molecular and spectral

```json
{
  "modality": "chemical",
  "applicability": "medium",
  "applicability_note": "Chemical character is present in systems with thermally reactive materials.",

  "byproducts": [
    {
      "category": "oxidation_products",
      "description": "Metal oxides characteristic of thermally stressed metal surfaces."
    },
    {
      "category": "combustion_byproducts",
      "description": "Volatile organic compounds characteristic of overheated polymers or insulation."
    }
  ],

  "olfactory_character": {
    "present": "conditionally",
    "character": "acrid, burning, smoky"
  },

  "spectral_signature": null
}
```

---

### 7.6 `electromagnetic.json` — EM spectrum

```json
{
  "modality": "electromagnetic",
  "applicability": "high",

  "primary_spectral_region": "thermal_infrared",
  "wavelength_range_um": {"low": 8, "high": 14},

  "emission_character": "blackbody_elevated",

  "secondary_spectral_regions": ["near_infrared"],
  "secondary_notes": "Near-infrared band shows elevated emission at pre-incandescent temperatures.",

  "rf_signature": null
}
```

---

### 7.7 Extensions — future modalities

Any additional perceptual modality may be added under `payload/perceptual/extensions/`.
The file must follow this envelope:

```json
{
  "modality": "<custom_name>",
  "clf_version": "0.9",
  "extension_author": "<issuer_id>",
  "applicability": "high|medium|low|not_applicable",
  "content": {
    // modality-specific fields — free structure
  }
}
```

Unknown modalities MUST be ignored by parsers that do not recognize them.
They MUST still be included in the `payload_hash_tree`.

---

## 8. `payload/schema/constraints.json` — necessary conditions (declarative)

This file describes the conditions that are DEFINITIONALLY necessary for something
to be an instance of this concept. These are NOT detection thresholds.

```json
{
  "constraint_language": "natural_language",

  "necessary_conditions": [
    {
      "id":          "NC-1",
      "statement":   "A physical system must exist and be in operation.",
      "type":        "existence"
    },
    {
      "id":          "NC-2",
      "statement":   "Temperature is a meaningful property of the system.",
      "type":        "property_presence"
    },
    {
      "id":          "NC-3",
      "statement":   "The system's thermal condition is characterized as thermally excessive.",
      "type":        "state_character"
    }
  ],

  "sufficient_conditions": null
}
```

---

## 9. `signatures/signature.json`

```json
{
  "concept_id":     "C-001.overheat",
  "version":        "1.0.0",
  "signed_at":      "2026-04-28T10:15:00Z",
  "signer_id":      "COLIBRI_DEMO",
  "algorithm":      "Ed25519",
  "container_hash": "sha256-<container_hash from manifest>",
  "public_key_id":  "colibri-demo-signing-key-2026-001",
  "signature":      "<base64url-encoded Ed25519 signature over container_hash>"
}
```

In production: `signature` is the Ed25519 (or RSA-PSS-4096) signature over the
UTF-8 bytes of the `container_hash` value from `manifest.json`.

---

## 10. Concept versioning rules

| Change type | Version bump | Rule |
|---|---|---|
| Correction of linguistic definition | PATCH | Must not change semantic scope |
| Addition of a new perceptual modality | MINOR | Existing fields unchanged |
| Addition of a new constraint | MINOR | Must not contradict existing constraints |
| Change of semantic scope (what the concept means) | MAJOR | Effectively a new concept |
| Deprecation | special | `maturity: deprecated`, forward pointer to replacement |

Version `1.0.0` = first stable release.
Versions below `1.0.0` are drafts and may be superseded without notice.

---

## 11. Concept ID format

```
<registry_prefix>-<sequential_number>.<name>
```

| Component | Rules |
|---|---|
| `registry_prefix` | Uppercase, 1–8 chars, identifies the issuing registry |
| `sequential_number` | Zero-padded integer, minimum 3 digits |
| `name` | lowercase_snake_case, describes the concept |

Examples:
- `C-001.overheat` — COLIBRI registry, concept 1
- `IEC-047.short_circuit` — IEC registry, concept 47
- `BIO-002.apoptosis` — Biological domain registry, concept 2

The combination `(registry_prefix, sequential_number)` is globally unique.
The `.name` is human-readable and informative, not normative.

---

## 12. What is NOT in the concept — the control layer boundary

The following belong EXCLUSIVELY to the control layer and MUST NOT appear
in any concept file:

| Category | Example | Why excluded |
|---|---|---|
| Numeric thresholds | T > 80°C → overheat | System-specific, not semantic |
| Detection algorithms | "Use FFT, then apply SVM classifier" | Intelligence, not meaning |
| Causal chains | "overheat causes insulation failure" | Inference, not definition |
| Actions/responses | "trigger alarm", "reduce load" | Control logic, not semantics |
| Taxonomic hierarchies | "overheat IS-A thermal_fault" | Ontology, not atomic concept |
| Probabilities/statistics | "overheat occurs 3× more in summer" | Epidemiology, not definition |
| Normative thresholds | "IEC 60034-1 defines overheat as..." | Standard reference, not concept |

The concept is a **mirror of reality**.
The control layer is the **eye that looks into the mirror and decides what it sees**.

---

## 13. Minimal valid concept (just text — always sufficient)

A valid concept can consist of only three files:

```
C-001.overheat.clf/
├── manifest.json      (with clf_version, concept_id, version, concept_kind, payload_hash_tree, container_hash)
├── taxonomy.json      (with concept_kind, domain, scope)
└── payload/
    └── definition/
        └── canonical.txt
```

Every other section enriches the concept across additional modalities.
All modalities are equal — linguistic and perceptual — and together they constitute the concept.

---

## 14. Design rationale — why each section exists

| Section | Rationale |
|---|---|
| `canonical.txt` | Language is the universal medium. Any concept must be expressible in words. |
| `i18n/` | Meaning is universal; language is local. Translations must not diverge semantically. |
| `visual.json` | Vision is the highest-bandwidth human sense. Many concepts have visual character. |
| `acoustic.json` | Sound is characteristic of many physical phenomena and states. |
| `signal.json` | Industrial and scientific systems are primarily understood through time-series data. |
| `haptic.json` | Touch and temperature perception are direct sensory experiences of physical states. |
| `chemical.json` | Many phenomena have chemical signatures (smell, composition, spectroscopy). |
| `electromagnetic.json` | The EM spectrum encompasses a wide range of physical signatures across many phenomena. |
| `extensions/` | Unknown future modalities must be accommodated without breaking existing parsers. |
| `constraints.json` | Captures what is definitionally necessary, distinguishing essence from threshold. |
| `signatures/` | Immutability requires cryptographic verification. |

---

*CLF Specification v0.9 — COLIBRI Concept Library Project — 2026-04-30*

---

© 2026 Pekka Lepola. All rights reserved.
Protected by Finnish Utility Model No. 13913.
No part of this specification may be used, reproduced, or implemented
without explicit written permission from the rights holder.
