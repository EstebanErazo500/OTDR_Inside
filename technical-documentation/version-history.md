<h1 align="center">Version history · reconstructed engineering record</h1>

<p align="center">
  <a href="../README.md"><img src="../assets/nav/project-home.svg" alt="Project home"></a>
  <a href="README.md"><img src="../assets/nav/technical-docs.svg" alt="Technical docs"></a>
  <img src="../assets/nav/version-history.svg" alt="Version history · current page">
  <a href="architecture.md"><img src="../assets/nav/architecture.svg" alt="Architecture"></a>
  <a href="event-provenance.md"><img src="../assets/nav/event-provenance.svg" alt="Event provenance"></a>
  <a href="validation.md"><img src="../assets/nav/validation.svg" alt="Validation"></a>
  <a href="data-handling.md"><img src="../assets/nav/data-handling.svg" alt="Data handling"></a>
</p>

<p align="center">
  <a href="version-history.md"><img src="../assets/nav/lang-en-selected.svg" alt="English"></a>
  <a href="version-history.es.md"><img src="../assets/nav/lang-es.svg" alt="Español"></a>
</p>

> This public history is reconstructed from archived development snapshots. It documents the real sequence of technical changes, but it does **not** pretend those snapshots were originally Git commits. The timeline is documentary: no source-code tags or downloadable releases are planned under the current publication permission.

## Evolution at a glance

| Version | Engineering focus | Main change |
|---|---|---|
| **v0.1.0** | Functional vertical slice | Safe SOR scanning, profile-based interpretation, trace reconstruction, local viewer, stored EXFO events and JSON/CSV export. |
| **v0.1.1** | Runtime robustness | Startup diagnostics, Python checks and local-port fallback without changing the analysis core. |
| **v0.1.2** | Security-compatible launch | Shell launchers removed in favor of a VS Code task compatible with Smart App Control. |
| **v0.2.0** | Evidence-backed Ceyear semantics | Curve characterization, capability-level support and explicit raw/normalized separation. |
| **v0.3.0** | Calculated event candidates | Curve-derived Ceyear candidates introduced as a source separate from stored events. |
| **v0.3.1** | Evidence and human review | Independent evidence windows and pending/accepted/rejected review state. |
| **v0.3.2** | Contextual hybrid detector | Persistence, polarity, recovery context and explainable suppression. |
| **v0.3.3** | EI/SOR multi-source analysis | Defensive EI reading, content-based pair verification and separate EI provenance. |
| **v0.3.4** | Terminal diagnostics | Navigable terminal region and multiscale localization experiments. |
| **v0.3.5** | Terminal evidence | Possible non-reflective-end candidate only when several terminal evidence conditions agree. |
| **v0.3.6** | EI/SOR consistency | Numerical/discretization consistency, duplicate-proposal correction and three-scale localization for weak responses. |
| **v0.4.0** | Multi-file workspace | Up to eight tabs, explicit EI association, 2–4 trace comparison, organized diagnostics and stricter terminal competition. |
| **v0.4.1** | Comparator correction | All-pairs proximity grouping, common/full span views and robust 5–50 m visual baseline; detector unchanged from v0.4.0. |
| **v0.4.2** | Evidence and experimental localization | Suppressed-candidate audit, terminal sensitivity, optional class-aware edge locator and improved reporting. |
| **v0.4.3** | Yokogawa and explainable morphology review | Profile-based SOR event interpretation, source-confirmed files kept distinct from instrument-declared metadata, and a Random Forest morphology advisory layer. |
| **v0.4.4** | JDSU MSOR and revisable associations | JDSU two-wavelength container support, per-trace provenance, richer native events, comparison review and evaluated adaptive localization. |
| **v0.4.5** | SOZ, comparative measurement and contextual review | Multitrace SOZ, source-aware comparisons, selectable IOR framing, event separations, local LSA loss estimates and RF/LSA review with retained provenance. |

## Phase 1 · stable structural baseline

### v0.1.0

The first archived release was already a complete vertical slice rather than a parser-only experiment. It combined read-only SOR loading, structural scanning, evidence-based profile selection, EXFO as the validated reference profile, trace reconstruction, interactive viewing, stored-event display and export.

The architectural choice established here survives every later snapshot:

`SOR → structural scanner → profile/normalization → trace → viewer`

The archived release records 23 automated tests passing.

### v0.1.1

The analysis core remained effectively unchanged. Development moved to startup diagnostics, explicit Python-version checks, visible failure handling and fallback to alternate local ports.

### v0.1.2

The next change was deployment-related. Shell launchers were removed and startup moved to an integrated VS Code task so the workflow could coexist with Smart App Control without weakening operating-system security.

## Phase 2 · vendor semantics become explicit

### v0.2.0

Ceyear support moved from preliminary structural support to an empirically characterized curve profile. The release introduced capability-level support, evidence/confidence around normalized fields and a deliberate distinction between nominal range and sampled extent.

This is where the project formally separates **reading bytes** from **claiming meaning**. Reconstructing a curve does not automatically authorize every event or optical magnitude visible in vendor software.

## Phase 3 · calculated events with evidence and review

### v0.3.0

The 0.3 line introduced an experimental detector over the reconstructed Ceyear curve. Calculated candidates were kept distinct from events stored in the original SOR, and diagnostic quantities remained relative rather than being presented as certified physical magnitudes.

### v0.3.1

Instead of replacing the baseline detector because alternative thresholds produced different results, the release kept the generator and added an independent evidence layer plus explicit human review state.

That separated three questions in the data model:

- what the detector proposed;
- what evidence surrounds the proposal;
- what a reviewer decided.

### v0.3.2

The detector became contextual: polarity, persistence, recovery behavior and terminal/noise-region logic were added. Suppressed candidates kept their rule and feature context instead of disappearing silently.

The archived development comparison reports improved recovery of the available intermediate references while explicitly noting that those references were not blind evaluation data.

## Phase 4 · paired formats and terminal reasoning

### v0.3.3

A defensive `.EI` reader created a second input path. EI/SOR pairing stopped relying on filename resemblance and required the expected sample relationship plus matching acquisition metadata for the characterized family.

EI records became their own provenance source rather than being flattened into SOR-stored or calculated events.

### v0.3.4

The release added a navigable terminal diagnostic region and multiscale localization experiments. The multiscale spread was explicitly treated as algorithm sensitivity rather than a confidence interval.

### v0.3.5

Terminal reasoning gained a separate evidence stage. A terminal transition could become a **possible non-reflective-end candidate** only when the implemented edge/drop/noise evidence aligned. Otherwise, the region remained diagnostic.

The provenance did not change: a promoted terminal result remained calculated.

### v0.3.6

This release is a stabilization step rather than a new provenance model. It focuses on:

- consistent discretization between equivalent EI and SOR curves;
- duplicate/supplementary proposal selection;
- a three-scale alternative for weak responses that are inconsistent across scales;
- preservation of the v0.3.5 non-reflective-end logic.

The archived validation records **80 release-validation tests**, 18 controlled EI/SOR pairs, 72/72 post-start event associations and matching candidate classes/indices across those pairs. The reported mean absolute SOR-versus-EI position difference changed only marginally, so the documented benefit is **format consistency and a targeted localization improvement**, not a global accuracy claim.

Across 27 private field SOR traces, 22 retained candidate indices/classes and 5 changed. Those changes remained subject to review because independent EI references were not available for every field trace.

## Phase 5 · multi-file workspace and comparison

### v0.4.0

v0.4.0 changes the way the analyzer is used without discarding the per-file engine.

The release adds:

- up to **eight independent file tabs**;
- per-tab zoom, selected event, annotations and review state;
- explicit **Associate EI to this SOR** behavior, with server-side content/metadata verification;
- comparison of **two to four traces**;
- acquisition-condition summary, curve overlay, event proximity table and comparison CSV;
- reorganized diagnostics: readable data, actionable findings, warnings and expandable technical detail;
- a stricter terminal-competition rule for late rises: a possible reflective end requires a local response at two pulse-related scales above three times local noise.

The release validation exercised 27 private field SOR traces and 18 controlled EI/SOR pairs. The 18 pairs preserved 72/72 associations with no extra unmatched calculated candidates. Relative to v0.3.6, only one private field trace changed terminal class under the stricter rule.

The original v0.4.0 comparator still had two presentation weaknesses: event matching was anchored to the first selected file, and the overlay used the first displayed sample as its vertical baseline.

### v0.4.1

v0.4.1 corrects the comparator while deliberately **not changing the detector**.

The new comparison behavior:

- groups nearby positions across **any pair** of selected files, not only against the first selection;
- keeps at most one event from each file in a row and retains unmatched events;
- provides **Common span** and **Full span** views when trace lengths differ;
- uses the median recovered level between **5 and 50 m** as the visual baseline instead of a potentially anomalous first sample;
- visually attenuates the terminal noise region for comparison display only.

The 15 m proximity threshold remains a review aid, not a physical tolerance or proof of event identity. The grouping is greedy and intentionally does not claim optimal physical association.

The archived v0.4.1 validation records **54 Python tests passing with 6 external-corpus tests skipped**, plus simulated DOM/canvas coverage of the corrected comparison behavior. At the time of that validation, a final visual inspection of the revised comparator in a Windows browser was still pending.

### v0.4.2 · documented revisions of the same release

**29 September — comparison audit.** Visible events gain evidence grades, and nearby suppressed proposals can be inspected with their exclusion rules. Suppressed items are not counted as event matches. A ±10% minimum-noise-threshold diagnostic flags unstable non-reflective-end positions; it does not alter the baseline detector's positions or classes.

**1–8 October — optional class-aware edge localization.** An experimental method examines local transitions by event class, preserving baseline positions for comparison and reversal. It is **off by default**. On 72 events from 18 controlled EI/SOR pairs, retrospective mean absolute difference improves against the EXFO study reference (4.399 → 2.726 m) but worsens against acquisition-paired EI (3.782 → 5.532 m). These are concordance comparisons, not verified physical distance errors; the EXFO/Ceyear measurement identity is not established.

**8 October — presentation refinement.** Sessions retain the chosen method and history; the local viewer gains consolidated session/report actions and revised vector PDF layout. The archived report records 92 Python tests passed with available corpora and simulated interface checks; a real Windows-browser inspection was not completed there.

v0.4.2 is not presented as a final release or as a certified improvement in positioning. Its value is explicit diagnostic evidence, reversible experimentation and clearer reports.

## Phase 6 · more vendor formats and measurement-aware review

### v0.4.3 · Yokogawa profile and evidence-based identity

The 0.4.3 line extends Yokogawa SOR analysis beyond structural readability: characterized native `KeyEvents` can be shown with their recorded positions and available optical magnitudes. It also introduces a **Random Forest morphology advisory** over event-centered signal windows; the model suggests signal-shape categories without rewriting stored event positions, loss or provenance. Its scores are **not calibrated probabilities or physical validation**.

The corrected **15 October revision 1** distinguishes two cases: 24 SOR files explicitly identify Yokogawa AQ1000 in their metadata; another 24 files from the same archived lot have their provenance confirmed by the project user but do **not** declare a model. Their manufacturer attribution is limited to those exact verified contents, not inferred from a generic software signature or filename. The archived regression accounts for **48 SOR files and 125 native event records**, maintaining original metadata. No JDSU MSOR reader was part of that 4.3 revision.

### v0.4.4 · JDSU MSOR and accountable comparison

v0.4.4 adds a defensive reader for the **observed JDSU T-BERD 6000A / 8126 MSOR VSRE 2.10** family. A container can expose two wavelength-specific traces, each with its own raw samples, native event table and source context; missing fields stay unavailable rather than being fabricated. External TXT references are tied to verified original data, not generalized to arbitrary incoming files.

It expands cross-device comparison with reference selection, explicit **one-to-one correspondence review**, unmatched events, acquisition context, source-aware CSV/PDF and saved sessions. A proximity proposal as wide as **50 m** helps a human inspect potential pairs but is not an instrument tolerance or proof of physical identity. Comparing opposite wavelengths as if they were the same acquisition is not presented as a valid aggregate.

The 4.4 adaptive edge option was explicitly **under evaluation**. In the documented controlled retrospective study, concordance improved against EXFO and JDSU, while it worsened against paired EI and regressed on 14 EXFO comparisons. These competing outcomes are preserved; the class-aware experiment is not treated as a physical accuracy certification.

### v0.4.5 · multiformat workspace and locally estimated event loss

The archived 0.4.5 release, consolidated **5 November 2026**, preserves the 4.4 positioning approach while extending the analysis and operator workflow:

- **SOR, EI, MSOR and SOZ** are supported for the characterized layouts. A multitrace SOZ appears in one container tab with its 1310/1550-nm members selectable; native events, raw samples and trace-level provenance are preserved.
- Matching SOR/SOZ copies are recognized by compatible acquisition information and standard samples/events, **not counted as independent acquisitions**.
- EXFO and Yokogawa are used as **peer documentary references** for the controlled comparisons; JDSU supplies further corroboration. Selecting a reference controls a comparison coordinate choice and does not declare that instrument infallible.
- The comparison can display original IOR axes or a conditional reference-IOR re-expression. If an input lacks valid IOR, conversion is not silently applied. This preserves source coordinates and does not discover a true physical group index.
- **Inter-event separation differences** complement absolute position differences, with dependent intervals clearly described.
- A **local least-squares approximation (LSA)** estimates loss near non-reflective steps from six before/after window combinations. Native loss, calculated LSA, supporting windows, window sensitivity and disagreements remain separately visible in UI, CSV/JSON and PDF.
- The previously trained **Random Forest morphology model** remains unchanged. Its advisory review now uses local window evidence and sign/shape disagreements; a high model score never overrides an unstable window or a native field.
- Sessions and reports preserve method selection, reference/axis decisions, trace/container identity, review and report context. The 5 November consolidation improves operator presentation and an IEC-oriented measurement **record**, without claiming IEC certification.

**Archived evidence and limits.** Nine SOZ files yielded 18 wavelength traces and 69 stored native events. For the controlled Ceyear corpus, 54 local splice-loss estimates were produced, with **49 supported and 5 marked for review**. Compared retrospectively with grouped EXFO references, mean absolute LSA differences were **0.0087 dB at 1310 nm** and **0.0202 dB at 1550 nm**; the corresponding Yokogawa comparison (for the covered 5- and 8-km events) was **0.0025 dB** and **0.0064 dB**. These are dependent same-setup comparisons, not calibrated loss uncertainties or proof of generalization. Contradictory native readings stay visible rather than being averaged away.

The final archived 5 November review reports **123 Python tests passing with its available data** and simulated DOM checks. The package's fresh-extraction check separately passed 25 targeted tests, validated its manifest and rendered example reports. A real Windows/browser run was **not verified** in that environment. The alternative two-reference RF position selector was tested but **not activated** because gains in average position concordance came with worse inter-event separations and some worst cases.

**Work dated 6 November is tracked separately as proposals/isolated experiments**, not 4.5 shipped features: comparison vertical-centering and IOR-warning patch, direct EXFO `.stdr` ingestion, further RF/quality/comparability evaluation, and future event-association improvements. The standalone comparator patch passed isolated regressions but was not packaged into the original 4.5 delivery.

## What remained stable

Two baseline artifacts are **byte-identical across every archived snapshot from v0.1.0 through v0.4.1**:

- `scanner.py` — the structural SOR scanner;
- `exfo_ftb7200_sor2.json` — the validated EXFO reference profile.

That continuity is important: the project grew by adding semantics, evidence, EI support, event/terminal reasoning and workspace behavior around a stable structural contract rather than repeatedly rewriting the parser.

Another useful boundary appears in v0.4.1: `events_hybrid.py` is identical to v0.4.0. The comparison correction belongs to the workspace/presentation layer, not to the detector.

## Current public publication policy

The sequence above documents the **engineering releases**. It is not a promise of public source-code tags or installable GitHub Releases. The current authorization permits reviewed screenshots of the viewer, conceptual architecture, general/synthetic illustrations and public methodological summaries; it **does not authorize publishing the application source code**, even in sanitized or renamed form.

Original measurements (`.SOR`, `.EI`, `.otdr`), derived operational curves, field reports, private filenames/hashes, internal paths and vendor packages remain outside GitHub. Public demonstrations must use independent synthetic examples or separately reviewed screenshots. Archived validation numbers are documented evidence, **not a runnable public test suite**.

If rights to distribute code change, the project would need a separate explicit authorization and a new publication review before any code, source tags, binaries or open-source license are added. Until then, the version sequence is represented by documentation, not public source snapshots. See the <a href="data-handling.md">data handling policy</a>.

---

<p align="center">
  <a href="README.md"><img src="../assets/nav/technical-docs.svg" alt="Technical docs"></a>
  <a href="architecture.md"><img src="../assets/nav/architecture.svg" alt="Architecture"></a>
</p>
