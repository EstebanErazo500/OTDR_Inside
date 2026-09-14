<h1 align="center">Architecture evolution · v0.1.0 → v0.3.4</h1>

<p align="center">
  <a href="../README.md"><img src="../assets/nav/project-home.svg" alt="Project home"></a>
  <a href="README.md"><img src="../assets/nav/technical-docs.svg" alt="Technical docs"></a>
  <a href="version-history.md"><img src="../assets/nav/version-history.svg" alt="Version history"></a>
  <img src="../assets/nav/architecture-current.svg" alt="Architecture · current page">
</p>

<p align="center">
  <a href="architecture.md"><img src="../assets/nav/lang-en-selected.svg" alt="English"></a>
  <a href="architecture.es.md"><img src="../assets/nav/lang-es.svg" alt="Español"></a>
</p>

<p align="center"><strong>Read bytes first. Interpret only when the evidence supports it.</strong></p>

OTDR Inside did not jump directly to the v0.3.4 architecture. The current system emerged by preserving a stable structural SOR boundary and adding vendor semantics, event analysis, provenance and a second EI input path around that core. This page therefore shows both **how the architecture evolved** and **how the current v0.3.4 system is organized**.

## Architecture evolution

<p align="center">
  <img src="../assets/architecture/evolution-light.svg#gh-light-mode-only" alt="OTDR Inside architecture evolution in light mode" width="100%">
  <img src="../assets/architecture/evolution-dark.svg#gh-dark-mode-only" alt="OTDR Inside architecture evolution in dark mode" width="100%">
</p>

The nine archived releases do not represent nine architectural rewrites. Some releases hardened runtime behavior while deliberately leaving the analysis core untouched. The meaningful architectural transitions are:

| Version | Architectural state | What changed structurally |
|---|---|---|
| **v0.1.0** | Stable vertical slice | Established the primary chain: safe SOR scan → profile interpretation → normalization → trace reconstruction → local viewer. EXFO acted as the validated reference while Ceyear remained preliminary. |
| **v0.1.1** | Same analysis architecture | Added startup diagnostics, Python checks and local-port fallback. The SOR engine, profiles and trace path were not redesigned. |
| **v0.1.2** | Same analysis architecture | Replaced shell launchers with a VS Code task to coexist with Smart App Control. This changed deployment, not the core data flow. |
| **v0.2.0** | Semantic-evidence layer | Introduced capability-level support, explicit evidence/confidence and a stronger separation between raw structure and vendor-specific semantics. Structural readability stopped being treated as semantic support. |
| **v0.3.0** | Event-analysis branch | Added calculated curve candidates as a new branch downstream of trace reconstruction while keeping them separate from events stored in SOR. |
| **v0.3.1** | Detection / evidence / review split | Added an independent evidence layer and explicit human-review state instead of collapsing detection and validation into one result. |
| **v0.3.2** | Contextual hybrid analysis | Extended event analysis with persistence, polarity, recovery context and terminal-region logic while retaining suppressed candidates and reasons. |
| **v0.3.3** | Multi-source architecture | Added a defensive EI reader and a second input path. EI/SOR pairing became content- and metadata-based, and EI records gained their own provenance. |
| **v0.3.4** | Current reference architecture | Added terminal D1 diagnostics and multiscale localization around the existing provenance-aware model without replacing the stable structural scanner. |

This evolution is important because it shows what remained stable as clearly as what changed. `scanner.py` and the validated EXFO profile are byte-identical across the archived snapshots from v0.1.0 through v0.3.4; later work expanded interpretation and analysis around that boundary instead of repeatedly rewriting it.

## Current reference architecture · v0.3.4

The diagram below represents the current **v0.3.4** architecture reconstructed from the archived implementation. It is the reference architecture for the public code migration.

<p align="center">
  <img src="../assets/architecture/system-light.svg#gh-light-mode-only" alt="OTDR Inside v0.3.4 architecture in light mode" width="100%">
  <img src="../assets/architecture/system-dark.svg#gh-dark-mode-only" alt="OTDR Inside v0.3.4 architecture in dark mode" width="100%">
</p>

## System at a glance

The current analyzer has two related input paths:

- **SOR path** — a SOR file is scanned safely, interpreted through evidence-backed profiles, reconstructed as a trace and, when the characterized profile allows it, analyzed for calculated event candidates.
- **EI path** — an EI file is read defensively using the observed CE6422 layout. It can be inspected on its own, or paired with a SOR only after content and acquisition metadata checks succeed.

Both paths converge in a **provenance-aware view model**. The viewer does not flatten everything into a single undifferentiated event table: stored SOR information, verified EI imports, calculated candidates and manual review remain distinguishable.

The user interface is served locally and the original measurements are not rewritten.

## Processing paths

### SOR path

1. **Structural scan** reads the SOR map, block boundaries, revisions and raw metadata while preserving block names and raw values.
2. **Profile and normalization** applies vendor-aware semantics only when the available evidence matches a characterized profile. Unsupported fields remain unresolved rather than being guessed.
3. **Trace reconstruction** derives the distance axis and relative trace level from the serialized sample data. Serialized samples and any recognized padding remain available in the analysis model.
4. **Event analysis** is kept separate from stored events. The Ceyear development line uses a deterministic baseline detector, contextual refinement, independent evidence audit and experimental localization diagnostics.

### EI path

1. **Defensive EI reading** validates the observed layout and preserves raw regions before interpreting the event tail.
2. An EI opened alone may expose its observed curve and imported records, but the session explicitly states that no SOR pairing has been verified.
3. **EI/SOR pairing is content-based**, not filename-based. The characterized Ceyear pair requires the expected sample relationship together with matching acquisition metadata before EI information is accepted as the companion source.
4. EI-derived records retain their own origin and do not overwrite SOR-stored or curve-calculated information.

### Presentation path

`viewer_model.py` orchestrates the structural scan, normalization, trace, event analysis and optional EI companion into a single model for the local viewer. The web layer renders that model and produces analysis/event exports without modifying the source measurement.

## Implementation map

The names below correspond to the archived v0.3.4 implementation. The sanitized public migration may reorganize package paths or public-facing identifiers, but these responsibility boundaries should remain intact.

| Layer | Archived module(s) | Responsibility | Deliberate boundary |
|---|---|---|---|
| **Local application shell** | `visor_otdr.py`, `web/*` | Local HTTP interface, file intake, static UI and downloads. | Operates locally; source measurements are handled through temporary copies rather than rewritten. |
| **Resource resolution** | `resources.py` | Resolves project, web and profile resources independently of the launch directory. | No measurement semantics. |
| **Structural parsing** | `scanner.py` | Walks SOR structure, validates bounds and exposes raw block metadata. | Does not turn structural readability into vendor semantic certainty. |
| **Profile interpretation** | `normalizer.py`, `profiles/*.json`, `export_signature.py` | Selects characterized profiles, normalizes supported fields and records evidence/confidence. | Raw values are retained; unsupported semantics remain explicit. |
| **Trace reconstruction** | `trace.py` | Reconstructs distance and relative trace level; identifies useful samples versus recognized serialized padding. | Relative level is not presented as universal calibrated optical power. |
| **Baseline event generation** | `events_baseline.py` | Produces deterministic curve candidates using robust local statistics. | Does not infer certified event loss, ORL, reflectance or physical fiber end. |
| **Context and evidence** | `events_hybrid.py`, `event_evidence.py` | Adds persistence, polarity, recovery context, terminal diagnostics and independent evidence windows. | Suppressed candidates remain explainable instead of disappearing silently. |
| **Localization diagnostics** | `localization.py` | Evaluates local step/ramp position at multiple scales. | Multiscale spread is algorithm sensitivity, not a statistical confidence interval. |
| **EI interpretation** | `ei_inspection.py`, `ei_reader.py` | Reads the observed EI layout defensively, preserves raw regions and imports supported records. | No EI writer; unknown variants are rejected instead of coerced. |
| **Application model** | `viewer_model.py` | Combines scan, normalization, trace, event sources, optional EI pairing and presentation state. | Event origin remains explicit throughout the payload. |

## Provenance is part of the architecture

OTDR Inside treats provenance as data, not as a note added at the end. The public model uses four conceptual origins:

| Origin | Meaning |
|---|---|
| **SOR stored** | Event or value serialized in the original SOR structure, such as a `KeyEvents` table when present. |
| **EI imported** | Record read from an EI file whose observed layout is supported; when shown as a verified companion, the SOR/EI pairing checks have passed. |
| **Calculated candidate** | Event hypothesis generated from the reconstructed curve by the analysis pipeline. |
| **Manual review** | User annotation or review state created during the analysis session. It does not modify the measurement. |

A value may be visible in the same interface as another value without having the same evidentiary status. That distinction is intentional and is preserved in exports.

## Architectural boundaries by vendor

| Ecosystem | Current role | Architectural consequence |
|---|---|---|
| **EXFO FTB-7200D** | Validated reference baseline | Stored events and characterized SOR 2.00 semantics can flow through the normalized model with high confidence for the validated profile. |
| **Ceyear CE6422** | Characterized active development line | Trace semantics, calculated candidates and the observed EI/SOR relationship are handled through explicit profile/signature checks and provenance. |
| **Yokogawa AQ1000** | Structural research scope | Structural readability is not promoted to a semantic profile until vendor-specific interpretation has enough evidence. |

Support is therefore progressive rather than binary:

<p align="center"><strong>structurally readable → profile identified → semantically characterized → empirically validated</strong></p>

## Architectural invariants

- **Read-only source handling.** Analysis must not rewrite the original measurement.
- **Structure before semantics.** The scanner establishes what is physically present before a profile assigns meaning.
- **Raw before normalized.** Normalization adds an interpreted layer without discarding the stored representation.
- **Stored before calculated.** Calculated candidates never masquerade as events serialized by the instrument or vendor software.
- **Unknown before guessed.** Unsupported variants degrade explicitly instead of receiving speculative values.
- **Pairing by evidence.** An EI/SOR pair is accepted by observed content and acquisition metadata, not by filename resemblance.
- **Deterministic analysis.** Given the same supported input and parameters, the event pipeline is designed to reproduce the same candidates and diagnostics.
- **Local presentation boundary.** The application serves its UI on the local machine and keeps operational measurements outside the public repository.

## What this architecture does not claim

The architecture should not be read as a claim of universal SOR compatibility or independent Telcordia certification. In particular, calculated Ceyear candidates do not create certified event loss, reflectance, ORL or fiber-end semantics. Likewise, the experimental localization spread is not a calibrated uncertainty interval, and an EI opened without a verified SOR companion is not silently treated as a confirmed pair.

Those limits are not missing features hidden by the interface; they are explicit boundaries of the evidence currently available to the project.

## Why this structure matters

The stable structural core allowed later work to add Ceyear semantics, event evidence, hybrid detection, EI support and terminal diagnostics without rewriting the SOR scanner. The architecture therefore reflects the project's central engineering choice: **extend interpretation around a stable raw-data boundary instead of coupling every vendor-specific rule directly to the parser**.

---

<p align="center">
  <a href="README.md"><img src="../assets/nav/technical-docs.svg" alt="Technical docs"></a>
  <a href="version-history.md"><img src="../assets/nav/version-history.svg" alt="Version history"></a>
</p>

<p align="center"><sub>Next standalone documentation page: Event provenance.</sub></p>
