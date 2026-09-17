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

> **Scope of this page.** This document reconstructs the architecture represented by the archived snapshots currently analyzed for the public repository, from **v0.1.0 through v0.3.5**. That range is a documentation boundary, not an endpoint of the project. Later releases extend this history instead of retroactively turning the latest documented snapshot into a final architecture.

OTDR Inside is best understood as a sequence of engineering decisions rather than one finished diagram. The project began with a stable structural SOR path and then added vendor semantics, explicit evidence, curve-derived event analysis, review state, EI as a second source, terminal diagnostics and finally a separate terminal-evidence stage.

This page is organized in three layers:

| Layer | What it answers |
|---|---|
| **Evolution map** | Which architectural capabilities appeared, and in what order? |
| **Release-by-release notes** | What changed in each archived version, what stayed stable, and why the change mattered? |
| **Detailed implementation snapshot** | How the modules present in v0.3.5 fit together. It is a detailed snapshot, not a claim that v0.3.5 is final. |

## Evolution map

<p align="center">
  <img src="../assets/architecture/evolution-light.svg#gh-light-mode-only" alt="OTDR Inside architecture evolution in light mode" width="100%">
  <img src="../assets/architecture/evolution-dark.svg#gh-dark-mode-only" alt="OTDR Inside architecture evolution in dark mode" width="100%">
</p>

The archived versions do not represent repeated rewrites. Several releases deliberately preserve the analysis core while strengthening execution, interpretation or diagnostics around it. The structural boundary established early in the project remains the anchor for later layers.

## Release-by-release architectural development

### v0.1.0 — Functional structural baseline

**Introduced.** Local read-only `.SOR` loading, structural scanning through the SOR map and named blocks, profile-based interpretation, normalization, trace reconstruction, interactive viewing, stored EXFO events and JSON/CSV export. EXFO provides the validated reference profile while Ceyear support is still preliminary.

**Architectural boundary.** Structural parsing and semantic interpretation are separate responsibilities from the beginning. The scanner establishes what is physically present; profiles and normalization decide what can be interpreted with evidence.

**Why it matters.** This release establishes the long-lived spine: `SOR → scanner → profile/normalization → trace → viewer`.

### v0.1.1 — Runtime hardening without redesigning the engine

**Introduced.** Startup diagnostics, explicit Python-version checks, visible failure reporting and fallback to additional local ports when the default port is unavailable.

**Preserved.** Scanner, profiles, normalization and trace reconstruction remain effectively unchanged.

**Why it matters.** Runtime concerns stay around the analyzer instead of leaking into measurement logic.

### v0.1.2 — Security-compatible launch workflow

**Introduced.** Shell launchers are removed and startup moves to an integrated VS Code task so the application can coexist with Windows Smart App Control without weakening operating-system security.

**Preserved.** The SOR engine, profiles and read-only behavior remain unchanged.

**Why it matters.** Deployment evolves independently from the parser and semantic pipeline.

### v0.2.0 — Evidence becomes part of semantic interpretation

**Introduced.** Ceyear curve behavior is characterized against reference software using controlled material. Support becomes capability-based rather than a single binary supported/unsupported flag; evidence and confidence accompany interpreted fields; nominal range and sampled extent are separated; recovered vertical level remains explicitly relative.

**Preserved.** Structural readability still comes from the same raw-data boundary, and unsupported event semantics are not invented merely because the curve can be reconstructed.

**Why it matters.** The architecture formally separates **reading bytes** from **claiming meaning**.

### v0.3.0 — Calculated event analysis becomes a separate branch

**Introduced.** A curve-based detector proposes peaks and persistent level transitions for the characterized Ceyear family. Serialized samples and the useful analysis region are distinguished; calculated candidates remain separate from event information stored in the SOR.

**Preserved.** Event analysis sits downstream of trace reconstruction. The parser and normalizer do not pretend that calculated candidates were serialized by the instrument or vendor software.

**Why it matters.** Event inference becomes an explicit analysis layer with its own provenance.

### v0.3.1 — Detection, evidence and human review are separated

**Introduced.** Independent evidence windows, local-noise/context measurements and explicit reasons are added without replacing the baseline generator simply because other thresholds produce different outputs. Human review receives its own `pending / accepted / rejected` state, comments and history.

**Preserved.** Automated generation and human validation remain different pieces of information.

**Why it matters.** The model now answers three separate questions: *What did the detector propose? What evidence surrounds it? What did a human reviewer decide?*

### v0.3.2 — Event analysis becomes contextual and explainable

**Introduced.** The hybrid detector adds polarity, persistence, recovery context and terminal/noise-region logic. A limited number of additional persistent-change proposals can be considered, and suppressed candidates retain the rule and feature vector that explain the decision.

**Preserved.** The baseline path remains the deterministic foundation while the contextual layer refines rather than silently erases results.

**Why it matters.** Explainability becomes part of the event model, including negative decisions.

### v0.3.3 — The architecture becomes multi-source

**Introduced.** A defensive `.EI` reader creates a second input path. EI/SOR pairing is content- and metadata-based rather than filename-based. EI information obtains its own provenance category alongside SOR-stored information, calculated candidates and manual review.

**Preserved.** An EI may be inspected without silently claiming a verified SOR pair, and EI-derived records do not overwrite other origins.

**Why it matters.** OTDR Inside moves from a single-file pipeline to a provenance-aware multi-source model.

### v0.3.4 — Terminal diagnostics and localization are added

**Introduced.** A navigable D1 terminal region, experimental multiscale localization, aligned CSV/JSON outputs and dedicated regression around terminal behavior, determinism and display reduction.

**Preserved.** The localization spread is explicitly algorithm sensitivity, not statistical uncertainty, and the experimental estimator does not automatically replace the primary event position.

**Why it matters.** Terminal behavior becomes a visible diagnostic layer without yet being promoted to a physical end candidate by default.

### v0.3.5 — Terminal evidence becomes a separate decision stage

**Introduced.** A new `terminal.py` stage evaluates whether a previously detected coarse noise transition has enough support to become a **possible non-reflective end** candidate. The evaluation combines three-window change-point agreement, persistent post-transition noise and a relative decline beyond the preceding trend. A reflective end already selected by earlier rules prevents the non-reflective path from taking over.

**Preserved.** If the evidence is incomplete, the region remains diagnostic D. A promoted end remains calculated and reviewable; it does not gain invented event loss, reflectance, ORL or certified physical-end semantics.

**Additional guard.** When a non-reflective end is supported, weak nearby level changes are compared against local preterminal noise instead of being blanket-excluded. At the same time, the established supplemental proposal-search domain is deliberately preserved so newly exposed terminal structure cannot displace earlier proposals because of a proposal-count cap.

**Why it matters.** The architecture now distinguishes **terminal detection**, **terminal evidence** and **terminal event representation**. That makes promotion from diagnostic region to event candidate auditable instead of being a hidden threshold side effect.

## Cross-version architectural thread

| Concern | Early state | How it expands |
|---|---|---|
| **Structure** | Safe SOR scanning in v0.1.0 | Remains the stable raw-data boundary through v0.3.5. |
| **Semantics** | Profile-based interpretation | Gains capability-level evidence, confidence and explicit unsupported states in v0.2.0. |
| **Events** | Stored EXFO events | Gains calculated candidates, evidence, contextual refinement, review and terminal-event reasoning. |
| **Sources** | SOR only | Adds defensive EI reading and evidence-based EI/SOR pairing in v0.3.3. |
| **Diagnostics** | Trace and stored-event visualization | Adds terminal-region reasoning in v0.3.4 and evidence-based promotion logic in v0.3.5. |

Two baseline components are byte-identical across the archived snapshots from v0.1.0 through v0.3.5: `scanner.py` and the validated EXFO profile. `normalizer.py` and `trace.py` are also unchanged between v0.3.4 and v0.3.5. The new release therefore extends terminal reasoning without moving the raw-data boundary.

## Detailed implementation snapshot · v0.3.5

The remainder of this page uses **v0.3.5 as the newest archived implementation snapshot currently analyzed for the repository**. It is not a final release or a stopping point. When later versions are incorporated, this snapshot can move forward while the earlier architecture record remains intact.

<p align="center">
  <img src="../assets/architecture/system-light.svg#gh-light-mode-only" alt="OTDR Inside v0.3.5 implementation snapshot in light mode" width="100%">
  <img src="../assets/architecture/system-dark.svg#gh-dark-mode-only" alt="OTDR Inside v0.3.5 implementation snapshot in dark mode" width="100%">
</p>

## System at a glance in this snapshot

The v0.3.5 implementation has two related input paths and one explicit terminal-evidence branch:

- **SOR path** — safe structural scan → evidence-backed profile interpretation → trace reconstruction → calculated event analysis.
- **EI path** — defensive EI reading → content/metadata pairing checks when a SOR companion is supplied.
- **Terminal-evidence branch** — starts from a coarse terminal/noise transition, evaluates independent support, and either emits a calculated non-reflective-end candidate or leaves the region as diagnostic D.

All three feed a **provenance-aware view model**. Stored SOR information, verified EI imports, calculated candidates and manual review remain distinguishable.

The interface is served locally and the original measurements are not rewritten.

## Processing paths

### SOR path

1. **Structural scan** reads the SOR map, block boundaries, revisions and raw metadata while preserving raw values.
2. **Profile and normalization** applies vendor-aware semantics only when the available evidence matches a characterized profile.
3. **Trace reconstruction** derives distance and relative level from serialized sample data while retaining raw/sample-boundary information.
4. **Event analysis** generates and contextually refines calculated candidates separately from stored event information.
5. **Terminal evaluation** examines a coarse terminal transition with independent change-point, decline and post-transition-noise evidence before any non-reflective end is promoted.

### EI path

1. **Defensive EI reading** validates the observed layout and preserves raw regions before interpreting supported records.
2. A standalone EI does not silently imply a verified SOR pair.
3. **EI/SOR pairing is content-based**, using the characterized sample relationship plus acquisition metadata.
4. EI-derived records retain their own origin and do not overwrite SOR-stored or curve-calculated information.

### Presentation path

`viewer_model.py` combines scan, normalization, trace, event analysis, terminal evidence and optional EI pairing into the local viewer model. The web layer renders that model and produces JSON/CSV outputs without modifying the source measurement.

## Implementation map

| Layer | Archived module(s) | Responsibility | Deliberate boundary |
|---|---|---|---|
| **Local application shell** | `visor_otdr.py`, `web/*` | Local HTTP interface, file intake, UI and downloads. | Source measurements are processed through temporary/local handling rather than rewritten. |
| **Resource resolution** | `resources.py` | Resolves project, web and profile resources independently of launch directory. | No measurement semantics. |
| **Structural parsing** | `scanner.py` | Walks SOR structure, validates bounds and exposes raw block metadata. | Structural readability does not become vendor semantic certainty. |
| **Profile interpretation** | `normalizer.py`, `profiles/*.json`, `export_signature.py` | Selects characterized profiles and records normalized values with evidence/confidence. | Raw values remain available; unsupported semantics remain explicit. |
| **Trace reconstruction** | `trace.py` | Reconstructs distance and relative trace level; identifies useful samples versus recognized serialized padding. | Relative level is not presented as universal calibrated optical power. |
| **Baseline event generation** | `events_baseline.py` | Produces deterministic curve candidates using robust local statistics. | Does not infer certified event loss, ORL, reflectance or physical fiber end. |
| **Contextual event analysis** | `events_hybrid.py` | Adds polarity, persistence, recovery context, terminal-neighborhood guards and explainable suppression. | Suppressed candidates remain traceable instead of disappearing silently. |
| **General event evidence** | `event_evidence.py` | Attaches evidence/review-oriented context to visible candidates, including terminal-candidate evidence. | Evidence does not rewrite origin or source data. |
| **Localization diagnostics** | `localization.py` | Evaluates local step/ramp position at multiple scales. | Multiscale spread is algorithm sensitivity, not statistical uncertainty. |
| **Terminal evidence** | `terminal.py` | Evaluates three-window noise change, persistent tail noise and relative decline before promoting a non-reflective-end candidate. | A supported candidate is still not a certified physical end or calibrated distance uncertainty. |
| **EI interpretation** | `ei_inspection.py`, `ei_reader.py` | Reads the observed EI layout defensively and imports supported records. | Unknown EI variants are rejected rather than coerced. |
| **Application model** | `viewer_model.py` | Combines all sources, candidates, evidence, optional EI pairing and presentation state. | Event origin remains explicit throughout the payload. |

## Provenance is part of the architecture

| Origin | Meaning |
|---|---|
| **SOR stored** | Event or value serialized in the original SOR structure, such as `KeyEvents` when present. |
| **EI imported** | Record read from a supported EI layout; when shown as a verified companion, pairing checks have passed. |
| **Calculated candidate** | Event hypothesis generated from reconstructed-curve analysis, including a supported non-reflective terminal candidate. |
| **Manual review** | User annotation or review state created during the analysis session; it does not modify the measurement. |

A value may be visible beside another value without having the same evidentiary status. That difference is intentional and preserved in exports.

## Architectural boundaries by vendor in the documented snapshot

| Ecosystem | Role | Architectural consequence |
|---|---|---|
| **EXFO FTB-7200D** | Validated reference baseline | Stored events and characterized SOR 2.00 semantics can flow through the normalized model for the validated profile. |
| **Ceyear CE6422** | Characterized active development line | Trace semantics, calculated candidates, terminal evidence and the observed EI/SOR relationship use explicit profile/signature/provenance checks. |
| **Yokogawa AQ1000** | Structural research scope | Structural readability is not promoted to a semantic profile until vendor-specific interpretation has enough evidence. |

<p align="center"><strong>structurally readable → profile identified → semantically characterized → empirically validated</strong></p>

## Architectural invariants

- **Read-only source handling.** Analysis must not rewrite the original measurement.
- **Structure before semantics.** The scanner establishes what is present before a profile assigns meaning.
- **Raw before normalized.** Interpretation adds a layer without discarding the stored representation.
- **Stored before calculated.** Calculated candidates never masquerade as serialized events.
- **Unknown before guessed.** Unsupported variants degrade explicitly instead of receiving speculative values.
- **Pairing by evidence.** EI/SOR pairs are accepted through observed content and metadata, not filename resemblance.
- **Diagnostic before promoted event.** A terminal transition remains diagnostic until independent evidence supports promotion.
- **Candidate before certification.** Even a supported non-reflective end remains a calculated candidate, not a certified physical endpoint.
- **Deterministic analysis.** The same supported input and parameters are intended to reproduce the same candidates and diagnostics.

## What the documented architecture does not claim

The documented architecture is not a claim of universal SOR/EI compatibility or independent Telcordia certification. Calculated Ceyear candidates do not create certified event loss, reflectance or ORL. The v0.3.5 non-reflective-end path does not convert algorithmic agreement into a metrological tolerance or proof of the physical fiber endpoint. Manufacturer UI thresholds observed during development are not copied into the detector unless their semantics can be demonstrated.

Those limits are part of the architecture: future versions may extend evidence, but historical claims stay tied to what each snapshot actually demonstrated.

## Design direction across the sequence

The recurring design choice is to **extend interpretation around a stable raw-data boundary instead of coupling every vendor-specific rule directly to the parser**. v0.3.5 follows the same pattern: the new terminal logic is introduced as an independent evidence stage rather than as a rewrite of the structural scanner or trace reconstruction.

---

<p align="center">
  <a href="README.md"><img src="../assets/nav/technical-docs.svg" alt="Technical docs"></a>
  <a href="version-history.md"><img src="../assets/nav/version-history.svg" alt="Version history"></a>
</p>

<p align="center"><sub>Next standalone documentation page: Event provenance.</sub></p>