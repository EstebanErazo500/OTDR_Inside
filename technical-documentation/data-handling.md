<h1 align="center">Data handling · publication boundaries</h1>

<p align="center">
  <a href="../README.md"><img src="../assets/nav/project-home.svg" alt="Project home"></a>
  <a href="README.md"><img src="../assets/nav/technical-docs.svg" alt="Technical docs"></a>
  <a href="version-history.md"><img src="../assets/nav/version-history.svg" alt="Version history"></a>
  <a href="architecture.md"><img src="../assets/nav/architecture.svg" alt="Architecture"></a>
  <a href="event-provenance.md"><img src="../assets/nav/event-provenance.svg" alt="Event provenance"></a>
  <a href="validation.md"><img src="../assets/nav/validation.svg" alt="Validation"></a>
  <img src="../assets/nav/data-handling-current.svg" alt="Data handling · current page">
</p>

<p align="center">
  <a href="data-handling.md"><img src="../assets/nav/lang-en-selected.svg" alt="English"></a>
  <a href="data-handling.es.md"><img src="../assets/nav/lang-es.svg" alt="Español"></a>
</p>

<p align="center"><strong>Explain the engineering. Demonstrate the interface. Protect the source code and operational measurements.</strong></p>

## Purpose and current authorization

OTDR Inside is documented publicly as an engineering and research portfolio. **The publication boundary currently communicated for this project permits reviewed viewer screenshots, high-level architecture explanations and generic/synthetic trace illustrations; it does not permit publication of the viewer's source code.** That distinction is intentional. Public access to this repository is **not** permission to copy, reverse engineer, redistribute or license the unpublished implementation.

This policy applies to the public GitHub repository and its entire history, including branches, pull requests, attachments, tags, release assets and automated artifacts. Additional publication rights must be confirmed before the boundary changes.

<p align="center">
  <img src="../assets/data-handling/boundary-light.svg#gh-light-mode-only" alt="Public material separated from review-required and excluded project material, light theme" width="100%">
  <img src="../assets/data-handling/boundary-dark.svg#gh-dark-mode-only" alt="Public material separated from review-required and excluded project material, dark theme" width="100%">
</p>

## Classification matrix

| Material | Current decision | Practical rule |
|---|---|---|
| Project objectives, version history and aggregate methodology | **Publish** | Describe what each archived version introduced; distinguish observations from claims and unresolved limits. |
| Conceptual architecture and functional/data-flow diagrams | **Publish after content review** | Explain responsibilities and interfaces without exposing proprietary implementations, internal credentials or confidential design details. |
| Viewer screenshots | **Allowed after per-image review** | Remove or replace all operational names, metadata, paths, identifiers, private annotations and sensitive trace content. Permission to show the UI does not clear everything visible inside it. |
| Generic diagrams and entirely synthetic trace illustrations | **Publish** | Create independently for demonstration; label them as illustrative/synthetic and never describe them as validated field measurements. |
| Aggregated validation results | **Publish after review** | State sample size, conditions, references, omissions and limits; do not expose per-file identifiers, fingerprints, locations or sensitive measurements. |
| Real SOR, EI, OTDR, CSV/JSON exports, sessions, reports and derived curves | **Do not publish** | These can reproduce operational measurements or reveal identifiable metadata even when filenames are removed. |
| Original or adapted application source code, tests, build scripts, profiles and internal configurations | **Do not publish** | No current authorization to distribute the implementation. A cleaned or renamed source file is still source code. |
| Vendor software, DLL/EXE packages, proprietary manuals and licensed standards | **Do not redistribute** | Describe publicly available concepts only; link to legitimate vendor sources if appropriate. |
| Customer, route, team and device identifiers; serials; private filenames/hashes; absolute paths | **Do not publish** | These may also appear in tooltips, print views, exports, hidden SVG text, image metadata and Git history. |

**Default for an uncertain item:** keep it out of GitHub until its content and publication permission have both been checked. This repository does not contain a private area.

## Approved demonstration formats

### 1. Screenshots of the actual viewer

A screenshot may show the application layout and interaction: trace panel, comparison controls, tabs, event-provenance badges, diagnostics or reporting view. Before publication, examine the **entire captured region**, including browser chrome, tooltips, file paths, title bar, taskbar, side panels and report headers. Replace sensitive values using an independently prepared demonstration session where possible; visually masking text on a screenshot is a fallback, not proof that other hidden metadata is absent.

Keep screenshots representative of **an actual version**. If labels or trace data have been replaced, note that in the caption; do not imply an edited screenshot is an untouched operational measurement.

### 2. Generalized or synthetic curves

Prefer curves created independently from mathematical/synthetic scenarios (e.g., step, reflection, terminal noise) or a non-measurement schematic. They should **not** be cropped, rescaled, re-sampled or lightly modified copies of private traces. A real fiber topology, event-position sequence or measurement record may remain recognizable after those transformations.

Label synthetic images clearly: *“Illustrative synthetic trace — not field data or a physical accuracy reference.”*

### 3. Architecture and validation summaries

Conceptual layers such as SOR/EI ingestion, profile-aware interpretation, trace analysis, provenance, workspace and comparison can be shown. Internal formulas, proprietary rules, implementation listings or complete module source are not necessary for explaining the design.

Validation can be summarized in aggregate with the test conditions and caveats attached. For example, agreement with a vendor file or another viewer must not be renamed “physical distance accuracy” without an independent traceable reference.

## Pre-publication check

Before adding or updating any material:

1. **Permission:** is this type of material explicitly within the current allowed scope? Source code is not.
2. **Content:** remove or avoid customers, sites, routes, people, identifiers, serials, real filenames, hashes, internal URLs and file-system paths.
3. **Measurements:** confirm there is no original trace, event list, raw sample table, exported report or reconstructable operational measurement.
4. **Hidden data:** inspect image/EXIF metadata, PDF properties and embedded attachments, SVG source text, comments, accessible labels, hyperlinks and accidentally included alternate layers.
5. **Scientific wording:** label synthetic examples; distinguish stored/imported/calculated events and comparison concordance from calibrated accuracy.
6. **Staged diff:** inspect every new/modified file and its source bytes, not just the rendered GitHub page.
7. **History:** remember that deletion from the working tree does **not** remove previously published Git blobs or cached copies. Stop sharing and escalate any suspected exposure for containment and history remediation.
8. **Final review:** verify rendered images in both GitHub light/dark themes, internal navigation and captions before considering the publication complete.

A `.gitignore` helps prevent common accidents but is **not a confidentiality control** and cannot override explicit file additions or remove earlier commits. A private branch in a **public** repository is not a secure storage location for confidential source files.

## What the repository intentionally does not provide

This repository currently offers **documentation, conceptual diagrams and approved demonstrations**, not a downloadable or buildable copy of the proprietary viewer. It makes no claim that private regression tests can be executed from the public repository. **Do not create source-code releases, build artifacts, executable packages or open-source licenses for the viewer without a new explicit authorization and rights review.**

Future additions may include a curated, captioned screenshot gallery and original synthetic illustrations. Any change to the source-code publication decision must be deliberate and separately authorized; it is **not** implied by new software versions or by publishing their documentation.

## Change control

For each public-facing addition, retain a small private publication record: item type, version represented, content reviewer, permission basis, redactions made and date. Do not post the private approval correspondence, raw operational evidence or confidential identifiers as proof.

If information is published accidentally, stop further sharing, remove direct public access where possible and coordinate prompt remediation with the appropriate project stakeholders. Merely deleting the file in a later commit is not enough.

---

<p align="center">
  <a href="README.md"><img src="../assets/nav/technical-docs.svg" alt="Technical docs"></a>
  <a href="validation.md"><img src="../assets/nav/validation.svg" alt="Validation"></a>
</p>
