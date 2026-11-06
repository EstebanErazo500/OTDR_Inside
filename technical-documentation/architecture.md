<h1 align="center">Architecture evolution · documented snapshots</h1>

<p align="center">
  <a href="../README.md"><img src="../assets/nav/project-home.svg" alt="Project home"></a>
  <a href="README.md"><img src="../assets/nav/technical-docs.svg" alt="Technical docs"></a>
  <a href="version-history.md"><img src="../assets/nav/version-history.svg" alt="Version history"></a>
  <img src="../assets/nav/architecture-current.svg" alt="Architecture · current page">
  <a href="event-provenance.md"><img src="../assets/nav/event-provenance.svg" alt="Event provenance"></a>
  <a href="validation.md"><img src="../assets/nav/validation.svg" alt="Validation"></a>
  <a href="data-handling.md"><img src="../assets/nav/data-handling.svg" alt="Data handling"></a>
</p>

<p align="center">
  <a href="architecture.md"><img src="../assets/nav/lang-en-selected.svg" alt="English"></a>
  <a href="architecture.es.md"><img src="../assets/nav/lang-es.svg" alt="Español"></a>
</p>

<p align="center"><strong>Read bytes first. Interpret only when evidence supports it. Compare without erasing source boundaries.</strong></p>

> **Scope.** This page reconstructs the architecture represented by archived snapshots from **v0.1.0 through v0.4.5**. That range is a documentation boundary, not the final project architecture. Later releases should extend the timeline instead of making v0.4.1 look like an endpoint.

OTDR Inside evolved by adding layers around a stable raw-data boundary. The project first separated SOR structure from vendor semantics; later versions added calculated events, evidence, EI as a second source, terminal reasoning and finally a multi-file workspace that compares traces without merging their origin.

## Evolution map

<p align="center">
  <img src="../assets/architecture/evolution-light.svg#gh-light-mode-only" alt="OTDR Inside architecture evolution in light mode" width="100%">
  <img src="../assets/architecture/evolution-dark.svg#gh-dark-mode-only" alt="OTDR Inside architecture evolution in dark mode" width="100%">
</p>

## Architectural development by release

### v0.1.0–v0.1.2 · structural spine and runtime shell

v0.1.0 establishes the durable per-file path:

`SOR → scanner → profile/normalization → trace → viewer`

Structural parsing and semantic interpretation are separate responsibilities from the beginning. v0.1.1 and v0.1.2 harden startup and deployment around that engine without redesigning the measurement path.

### v0.2.0 · evidence becomes part of interpretation

Ceyear curve semantics move into an evidence-backed profile layer. Raw values remain available beside normalized values, support becomes capability-specific, and readable structure is no longer treated as equivalent to semantic support.

### v0.3.0–v0.3.2 · event analysis becomes a downstream branch

Calculated candidates are generated **after** trace reconstruction. Stored events and calculated events therefore never need to share the same source category.

v0.3.1 splits detector output, evidence and human review. v0.3.2 adds contextual refinement and preserves reasons for suppressed proposals.

### v0.3.3 · second input path

A defensive EI reader introduces a second measurement source. EI/SOR pairing becomes evidence-based and the imported EI information gets its own provenance instead of overwriting SOR or calculated results.

### v0.3.4–v0.3.6 · terminal reasoning and cross-format consistency

v0.3.4 adds a navigable terminal diagnostic region and localization experiments. v0.3.5 adds a terminal evidence gate for possible non-reflective-end candidates. v0.3.6 focuses on consistent discretization between equivalent EI/SOR curves, duplicate proposal selection and weak-response localization at three scales.

This phase strengthens analysis without changing the structural scanner or the basic provenance categories.

### v0.4.0 · workspace becomes a first-class architectural layer

The major architectural change in 0.4 is not a new parser. It is a **workspace around multiple independent analyses**.

v0.4.0 adds:

- up to eight per-file tabs;
- isolated zoom, selection, annotations and review state per tab;
- explicit EI association initiated from a SOR tab;
- comparison of two to four traces;
- comparison CSV and acquisition-condition context;
- reorganized diagnostics;
- a stricter terminal-competition rule in the detector.

The important boundary is that each file is still analyzed through the same per-file engine. The workspace stores and presents multiple view models; it does not collapse them into one measurement.

### v0.4.1 · comparison logic is corrected without changing the detector

v0.4.1 is intentionally a workspace/presentation refinement.

The detector module `events_hybrid.py` is byte-identical to v0.4.0. The significant changes live in the front-end workspace:

- proximity grouping is computed across any selected pair, not only relative to the first file;
- common-span/full-span views handle different trace lengths;
- the visual level baseline uses the 5–50 m median rather than the first sample;
- terminal noise is attenuated for comparison drawing only.

This makes a useful architectural point: **comparison policy can evolve independently from event detection**.

### v0.4.2 · evidence audit and reversible localization

The archived v0.4.2 documentation describes **successive changes under one release number**, not a new structural parser or a replacement of the multi-file architecture.

The comparator gains visible stability/evidence grades and access to nearby suppressed candidates with their rules; suppressed entries remain outside matched-event counts. A ±10% perturbation of the minimum terminal-noise threshold is used to **warn** about potentially unstable non-reflective-end locations. This diagnostic does not redefine the baseline event list.

A separately selectable `edge_precision.py` experiment can change calculated positions for supported classes while preserving the original positions, class, provenance and review history. The viewer defaults to the baseline method. EI-imported positions are not rewritten. Session and report payloads retain the method and associated comparisons.

This is a **localization experiment**, not a new validated source or a universal improvement: retrospective agreement with EI worsens even though agreement with an EXFO study reference improves.

<p align="center">
  <img src="../assets/architecture/v042-light.svg#gh-light-mode-only" alt="Documented v0.4.2 analysis, evidence and experimental localization in light mode" width="100%">
  <img src="../assets/architecture/v042-dark.svg#gh-dark-mode-only" alt="Documented v0.4.2 analysis, evidence and experimental localization in dark mode" width="100%">
</p>

### v0.4.3 · native-event profiles and morphology advisory

Yokogawa gains characterized **native SOR event semantics**, instead of remaining structural-only. Instrument-declared identity stays separate from user-confirmed provenance for files whose metadata does not name the instrument/model. The confirmation is content-specific and does not rewrite the original manufacturer field.

A **Random Forest morphological review layer** is added alongside the event model. It describes patterns around candidate or native events; it does **not** replace event origin, instrument-stored positions, reflectance or loss. A classifier score is not calibrated probability or physical ground truth.

### v0.4.4 · JDSU MSOR multi-wavelength container

The observed JDSU MSOR 2.10 family adds **container → subtrace** relationships: one file can hold two wavelength-specific measurement records. Each subtrace preserves its samples, native events, markers and provenance. Where a native field lacks demonstrated semantics, the model keeps its raw value or marks the derived value unavailable; verified TXT references are not extended to arbitrary files.

Cross-vendor comparison adds **revisable one-to-one event matching** and reference selection. A proximity proposal is a review candidate, not certified event identity. The adaptive Ceyear edge localization is kept explicitly as a method under evaluation because different reference sources disagree.

### v0.4.5 · SOZ, native-versus-estimated loss, and comparison context

The v0.4.5 consolidation adds a second multi-trace family, **Yokogawa SOZ**, with wavelength selection inside a container tab. SOR extracts associated with SOZ remain traceable to the same acquisition and must not inflate the independent sample count.

The comparison layer now records **reference selection, original/conditional common-IOR axes, absolute event positions and inter-event separations**. If an IOR is missing, requested re-expression is not silently applied. Reference choice does not change the original measurement or establish true physical IOR.

For suitable non-reflective events, the per-file analysis adds a **local LSA loss estimate** from before/after signal windows. Its value, six alternative window results and sensitivity/review indicators are stored separately from **native loss** and from the Random Forest **morphology suggestion**. These three quantities have different sources and uncertainty limitations.

Sessions/reports preserve container/subtrace identity, calculation method, comparison decisions, and review context. The report's IEC-oriented field inventory is documentation support, **not a calibration certificate**.

<p align="center">
  <img src="../assets/architecture/v045-light.svg#gh-light-mode-only" alt="Documented v0.4.3 to v0.4.5 architectural extensions, light theme" width="100%">
  <img src="../assets/architecture/v045-dark.svg#gh-dark-mode-only" alt="Documented v0.4.3 to v0.4.5 architectural extensions, dark theme" width="100%">
</p>

**After the 5 November package:** the isolated 6 November vertical-centering and IOR-notice patch, direct EXFO `.stdr` investigation and further AI/comparability proposals are research or unmerged work, not part of the shipped 4.5 package.

## Cross-version architectural thread

| Concern | Early state | Expansion through v0.4.1 |
|---|---|---|
| **Structure** | Safe SOR scanning | Stable raw-data boundary across all archived releases. |
| **Semantics** | Profile interpretation | Evidence, capability status and explicit unsupported states. |
| **Events** | Stored EXFO events | Calculated candidates, morphology advice, evidence, review and terminal reasoning; native event origins unchanged. |
| **Sources** | SOR only | EI, MSOR and SOZ characterized source/container paths with verified pair/copy relationships. |
| **Session state** | One active analysis | Up to eight independent tabs with per-file state. |
| **Comparison** | Manual alternation | 2–4 trace comparison with all-pairs visual proximity grouping. |
| **Diagnostics** | Basic alerts/details | Readable status, findings/actions, warnings and technical audit detail. |

Two baseline components are byte-identical across every archived snapshot from v0.1.0 through v0.4.1: `scanner.py` and the validated EXFO profile. The system therefore shows a long-lived structural contract while interpretation and interaction evolve around it.

## Implementation baseline · v0.4.1 (expanded through v0.4.5)

The diagram below is the **v0.4.1 per-file/workspace baseline**. Changes from 4.2 and 4.3–4.5 are represented in their separate diagrams and text above; no part is described as the final architecture.

<p align="center">
  <img src="../assets/architecture/system-light.svg#gh-light-mode-only" alt="OTDR Inside v0.4.1 implementation snapshot in light mode" width="100%">
  <img src="../assets/architecture/system-dark.svg#gh-dark-mode-only" alt="OTDR Inside v0.4.1 implementation snapshot in dark mode" width="100%">
</p>

## Processing model

### 1. Per-file SOR path

1. `scanner.py` validates SOR structure and exposes raw block data.
2. `normalizer.py` plus profiles add only supported vendor semantics.
3. `trace.py` reconstructs distance and relative level.
4. `events_baseline.py`, `events_hybrid.py`, `event_evidence.py`, `terminal.py` and `localization.py` derive calculated candidates and diagnostics while preserving their calculated origin.
5. `viewer_model.py` builds a per-file payload for presentation.

### 2. EI path and explicit association

`ei_reader.py` and `ei_inspection.py` interpret the observed EI family defensively. An EI can be opened as an independent tab. To use it as a companion, v0.4 requires the user to initiate association from a SOR tab; the existing content/metadata verification must succeed.

A verified companion changes the relationship between two files, not their provenance.

### 3. Multi-file workspace

The v0.4 workspace is primarily a front-end/session layer implemented in `web/app.js`:

- up to eight analyses may coexist;
- each tab keeps its own zoom, selected event, annotations and review state;
- closing a tab with unexported session changes requires confirmation;
- one bad file does not discard analyses that opened successfully.

The workspace stores **multiple independent view models**, not one merged trace.

### 4. Comparison layer

The comparator selects two to four open analyses.

In v0.4.1 it:

- groups visually nearby event positions across all selected files;
- preserves unmatched events;
- keeps at most one event from each file in a comparison row;
- offers common-span and full-span views;
- uses a 5–50 m median visual baseline;
- attenuates terminal noise for drawing only.

The comparator does not rewrite per-file results. It creates a **relationship view** between existing results.

## Implementation map

| Layer | Archived module(s) | Responsibility | Deliberate boundary |
|---|---|---|---|
| **Local server** | `visor_otdr.py` | HTTP interface, file analysis requests and static resources. | Local workflow; source measurements are not rewritten. |
| **Structural parsing** | `scanner.py` | Walk SOR structure, validate bounds, expose raw metadata. | Structural readability does not imply semantic certainty. |
| **Profiles / normalization** | `normalizer.py`, `profiles/*`, `export_signature.py` | Apply characterized semantics and keep evidence/confidence. | Raw values remain available. |
| **Trace** | `trace.py`, `numerics.py` | Reconstruct trace axes and numerical policies. | Relative level is not universal calibrated power. |
| **Event / terminal analysis** | `events_baseline.py`, `events_hybrid.py`, `event_evidence.py`, `terminal.py`, `localization.py` | Generate and audit calculated hypotheses. | No certified loss, reflectance, ORL or physical-end claim. |
| **EI interpretation** | `ei_reader.py`, `ei_inspection.py` | Read the observed EI layout and import supported records. | Unknown EI variants are not coerced. |
| **Per-file view model** | `viewer_model.py` | Combine structure, semantics, trace, event sources and optional EI companion. | Provenance remains explicit. |
| **Workspace** | `web/app.js`, `web/index.html`, `web/styles.css` | Tabs, session state, file switching, comparison UI and exports. | Multiple analyses coexist without being merged. |
| **Comparator** | v0.4.1 functions in `web/app.js` | Proximity groups, common/full span and visual baseline. | Proximity is a display relation, not physical identity. |
| **v0.4.2 evidence/localization** | `edge_precision.py` and documented UI/report changes | Suppressed-proposal explanations, terminal threshold sensitivity and optional class-aware position method. | Baseline detector and EI values are not silently overwritten. |

### Additions to the v0.4.5 conceptual map

| New responsibility | Characterized route | Boundary |
|---|---|---|
| **Yokogawa source/profile identification** | SOR native events; explicit versus externally confirmed provenance | Confirmation identifies specific files, not all files with similar fields. |
| **JDSU MSOR / Yokogawa SOZ** | Container → wavelength-specific subtrace → per-file analysis | One container or duplicate SOR/SOZ extraction is not multiple independent acquisitions. |
| **Morphology advice** | Event-centered samples → RF class suggestion → reviewer context | A model score is not calibrated probability, native event type or verified physical event. |
| **Local LSA loss** | Per-event before/after windows → local estimate + six-window sensitivity | Estimated and instrument-stored loss remain separate; sensitivity is not calibration uncertainty. |
| **Comparison / report context** | Peer reference, conditional IOR, absolute positions and inter-event separations | Re-expression does not rewrite source axes or certify measurement accuracy. |

## Architectural invariants

- **Read-only sources.** Original measurements are not rewritten.
- **Structure before semantics.** The scanner reports what exists before profiles assign meaning.
- **Raw before normalized.** Interpretation adds a layer instead of deleting the stored representation.
- **Stored/imported/calculated remain distinct.**
- **Pairing by evidence.** EI/SOR association is not inferred from filenames.
- **Per-file state remains isolated.** A workspace tab owns its own review and annotations.
- **Comparison does not mutate analysis.**
- **Proximity is not identity.** The 15 m comparison aid is not a metrological tolerance.
- **Visual normalization is not calibration.** Common/full span and 5–50 m baseline affect drawing, not physical measurement values.

## What this architecture does not claim

The v0.4.1 workspace makes cross-trace inspection easier; it does not make different files physically comparable under arbitrary acquisition conditions. Similar positions can occur in different links, wavelengths, pulses or group indices without representing the same element. The current proximity grouping is greedy and is not presented as an optimal event-association algorithm.

Likewise, EI agreement, EXFO agreement and terminal evidence remain engineering evidence under the documented conditions, not universal calibration or independent Telcordia certification.

This is a conceptual and historical architecture description. **Application source code and internal implementation files are not included in the public repository.** The <a href="data-handling.md">data handling policy</a> governs what may be demonstrated.

---

<p align="center">
  <a href="README.md"><img src="../assets/nav/technical-docs.svg" alt="Technical docs"></a>
  <a href="event-provenance.md"><img src="../assets/nav/event-provenance.svg" alt="Event provenance"></a>
  <a href="validation.md"><img src="../assets/nav/validation.svg" alt="Validation"></a>
</p>
