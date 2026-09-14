<h1 align="center">Architecture evolution · documented snapshots</h1>

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

> **Scope of this page.** This document reconstructs the architecture represented by the archived snapshots currently analyzed for the public repository, from **v0.1.0 through v0.3.4**. That range is a documentation boundary, not an endpoint of the project. Later releases should extend this history rather than treating v0.3.4 as a final architecture.

OTDR Inside is best understood as a sequence of engineering decisions rather than as one finished diagram. The project began with a stable structural SOR pipeline, then progressively added evidence-aware vendor interpretation, curve-derived event analysis, explicit provenance, human review and a second EI input path.

This page is organized in three layers:

| Layer | What it answers |
|---|---|
| **Evolution map** | Which architectural capabilities appeared, and in what order? |
| **Release-by-release notes** | What changed in each archived version, what remained stable, and why the change mattered architecturally? |
| **Detailed implementation snapshot** | How the modules present in the v0.3.4 snapshot fit together. This is a detailed example, not a claim that v0.3.4 is final. |

## Evolution map

<p align="center">
  <img src="../assets/architecture/evolution-light.svg#gh-light-mode-only" alt="OTDR Inside architecture evolution in light mode" width="100%">
  <img src="../assets/architecture/evolution-dark.svg#gh-dark-mode-only" alt="OTDR Inside architecture evolution in dark mode" width="100%">
</p>

The archived versions do not represent repeated rewrites. Several releases deliberately preserve the analysis core while strengthening execution, interpretation or diagnostics around it. The structural boundary established early in the project remains the anchor for the later layers.

## Release-by-release architectural development

### v0.1.0 — Functional structural baseline

**Introduced.** The first archived version already forms a complete vertical slice: local read-only `.SOR` loading, structural scanning through the SOR map and named blocks, profile-based interpretation, normalization, trace reconstruction, interactive viewing, stored EXFO events and JSON/CSV export. EXFO provides the validated reference profile, while Ceyear support is still preliminary.

**Architectural boundary.** Structural parsing and semantic interpretation are separate responsibilities from the beginning. The scanner establishes what is present in the file; profiles and normalization decide what can be interpreted with evidence.

**Why it matters.** This release establishes the long-lived spine of the project: `SOR → scanner → profile/normalization → trace → viewer`. Later work grows around this path rather than replacing it.

### v0.1.1 — Runtime hardening without redesigning the engine

**Introduced.** Startup diagnostics, explicit Python-version checks, visible failure reporting and fallback to additional local ports when the default port is unavailable.

**Preserved.** The SOR scanner, profiles, normalization and trace path remain effectively unchanged. The development focus shifts from analysis correctness to making the local application start and fail predictably in a Windows environment.

**Why it matters.** Runtime concerns become a layer around the analyzer instead of leaking into the structural or semantic core. The application shell can evolve independently from the measurement logic.

### v0.1.2 — Security-compatible launch workflow

**Introduced.** Shell launchers are removed and the local workflow moves to an integrated VS Code task so the application can coexist with Windows Smart App Control without asking the user to weaken operating-system security.

**Preserved.** The archived release explicitly leaves the SOR engine, profiles and read-only behavior unchanged.

**Why it matters.** Deployment is treated as an adapter around the analyzer. A platform-security constraint changes how the program is launched, not how OTDR measurements are parsed or interpreted.

### v0.2.0 — Evidence becomes part of semantic interpretation

**Introduced.** Ceyear CE6422 curve behavior is characterized against reference software using controlled material. Support becomes capability-based rather than a single binary supported/unsupported flag; evidence and confidence are attached to interpreted fields; nominal range and sampled extent are distinguished; recovered vertical level is kept explicitly relative.

**Preserved.** Structural readability still comes from the same scanning boundary. Raw values remain available even when a normalized meaning is added, and unsupported event semantics are not invented merely because the curve can be reconstructed.

**Why it matters.** This is the point where the architecture formally separates **reading bytes** from **claiming meaning**. Vendor-specific semantics become an evidence-backed layer instead of assumptions embedded in the parser.

### v0.3.0 — Calculated event analysis becomes a separate branch

**Introduced.** A curve-based experimental detector proposes peaks and persistent level transitions for the characterized Ceyear trace family. The implementation distinguishes the serialized sample region from the useful analysis region and keeps calculated candidates separate from event information stored in the SOR.

The viewer gains candidate selection and manual annotation support, while diagnostic quantities remain relative rather than being presented as certified loss, reflectance, ORL or fiber-end measurements.

**Why it matters.** Event analysis is added **downstream of trace reconstruction**. The parser and normalizer do not need to pretend that calculated events were serialized by the instrument or vendor software.

### v0.3.1 — Detection, evidence and human review are separated

**Introduced.** Instead of replacing the baseline generator simply because alternative thresholds produce different outputs, the release retains the baseline detector and adds an independent evidence layer: multiple context windows, local-noise measurements, neighboring context and explicit reasons. Human review receives its own `pending / accepted / rejected` state, comments and history.

**Preserved.** Automated generation remains distinguishable from human validation. Review state does not mutate the source measurement and does not retroactively turn a calculated candidate into a stored event.

**Why it matters.** The architecture now represents three separate questions: *What did the detector propose? What evidence surrounds the proposal? What did a human reviewer decide?* Those questions can evolve independently.

### v0.3.2 — Event analysis becomes contextual and explainable

**Introduced.** The hybrid detector adds polarity, persistence, recovery context and terminal/noise-region logic. A controlled number of additional persistent-change proposals can be considered, and suppressed candidates retain the rule and feature vector that explain why they were rejected.

**Preserved.** The baseline path remains available as a deterministic foundation, while the contextual layer refines rather than erases its outputs.

**Why it matters.** Event analysis becomes a layered decision system instead of a single threshold pass. Explainability is represented in the model, including negative decisions, not added afterward as prose.

### v0.3.3 — The architecture becomes multi-source

**Introduced.** A defensive `.EI` reader creates a second input path. EI/SOR pairing is no longer accepted by filename resemblance: the characterized family requires agreement between acquisition metadata and the expected complementary sample relationship before the EI is treated as a verified companion.

EI information receives its own provenance category alongside SOR-stored information, calculated candidates and manual review. An EI may also be inspected independently without silently claiming that a SOR pair has been verified.

**Why it matters.** This is the largest input-boundary change in the archived sequence. OTDR Inside moves from a single-file analysis pipeline to a provenance-aware model that can combine related sources without flattening their evidentiary status.

### v0.3.4 — Terminal diagnostics and localization are added

**Introduced.** The snapshot adds a navigable D1 terminal region, experimental multiscale localization diagnostics, aligned CSV/JSON outputs and dedicated regression around terminal behavior, determinism and display reduction.

The localization experiment explicitly treats multiscale spread as **algorithm sensitivity, not a statistical confidence interval**, and it does not replace the primary position simply because a more elaborate estimator exists.

**Why it matters.** Diagnostics grow around the existing provenance-aware event model without forcing the structural scanner or the EI/SOR boundary to change. v0.3.4 is simply the upper bound of the archived snapshots analyzed for this page; it is **not** presented as the final project architecture.

## Cross-version architectural thread

Across the documented sequence, the project follows a consistent pattern:

| Concern | Early state | How it expands |
|---|---|---|
| **Structure** | Safe SOR scanning in v0.1.0 | Remains the stable raw-data boundary across the archived sequence. |
| **Semantics** | Profile-based interpretation | Gains capability-level evidence, confidence and explicit unsupported states in v0.2.0. |
| **Events** | Stored EXFO events | Gains curve-calculated candidates, evidence, contextual refinement and review in v0.3.x. |
| **Sources** | SOR only | Adds defensive EI reading and evidence-based EI/SOR pairing in v0.3.3. |
| **Diagnostics** | Trace and stored-event visualization | Adds terminal-region reasoning and localization diagnostics by v0.3.4. |

Two baseline components are byte-identical across the archived snapshots from v0.1.0 through v0.3.4: `scanner.py` and the validated EXFO profile. That continuity is useful because it shows that later capability growth was layered around a stable structural contract.

## Detailed implementation snapshot · v0.3.4

The remainder of this page uses **v0.3.4 as a detailed implementation snapshot** because it is the most complete archived implementation currently included in this documentation set. It is not treated as a final release or as a stopping point for the architecture history. When later releases are incorporated, the evolution section should grow and the detailed snapshot can be updated without rewriting the earlier decisions.

<p align="center">
  <img src="../assets/architecture/system-light.svg#gh-light-mode-only" alt="OTDR Inside v0.3.4 implementation snapshot in light mode" width="100%">
  <img src="../assets/architecture/system-dark.svg#gh-dark-mode-only" alt="OTDR Inside v0.3.4 implementation snapshot in dark mode" width="100%">
</p>

## System at a glance in this snapshot

The v0.3.4 implementation has two related input paths:

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

The names below correspond to the archived v0.3.4 implementation. The sanitized public migration may reorganize package paths or public-facing identifiers, but these responsibility boundaries are the important part of the design.

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

OTDR Inside treats provenance as data, not as a note added at the end. The documented model uses four conceptual origins:

| Origin | Meaning |
|---|---|
| **SOR stored** | Event or value serialized in the original SOR structure, such as a `KeyEvents` table when present. |
| **EI imported** | Record read from an EI file whose observed layout is supported; when shown as a verified companion, the SOR/EI pairing checks have passed. |
| **Calculated candidate** | Event hypothesis generated from the reconstructed curve by the analysis pipeline. |
| **Manual review** | User annotation or review state created during the analysis session. It does not modify the measurement. |

A value may be visible in the same interface as another value without having the same evidentiary status. That distinction is intentional and is preserved in exports.

## Architectural boundaries by vendor in the documented snapshot

| Ecosystem | Role in the documented snapshot | Architectural consequence |
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

## What the documented architecture does not claim

The documented architecture should not be read as a claim of universal SOR compatibility or independent Telcordia certification. In particular, calculated Ceyear candidates do not create certified event loss, reflectance, ORL or fiber-end semantics. Likewise, experimental localization spread is not a calibrated uncertainty interval, and an EI opened without a verified SOR companion is not silently treated as a confirmed pair.

Those limits are explicit boundaries of the evidence represented by the archived implementations. Future versions may extend capabilities, but historical claims should remain tied to what each release actually demonstrated.

## Design direction across the sequence

The stable structural core allowed later work to add Ceyear semantics, event evidence, hybrid detection, EI support and terminal diagnostics without rewriting the SOR scanner. The recurring architectural choice is therefore to **extend interpretation around a stable raw-data boundary instead of coupling every vendor-specific rule directly to the parser**.

That design direction is expected to remain traceable as later releases are added to this documentation. New versions should extend the timeline rather than retroactively making v0.3.4 look like an endpoint.

---

<p align="center">
  <a href="README.md"><img src="../assets/nav/technical-docs.svg" alt="Technical docs"></a>
  <a href="version-history.md"><img src="../assets/nav/version-history.svg" alt="Version history"></a>
</p>

<p align="center"><sub>Next standalone documentation page: Event provenance.</sub></p>
