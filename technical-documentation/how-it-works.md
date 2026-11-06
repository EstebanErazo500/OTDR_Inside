<h1 align="center">How OTDR Inside works · Guided walkthrough</h1>

<p align="center">
  <a href="../README.md"><img src="../assets/nav/project-home.svg" alt="Project home"></a>
  <a href="README.md"><img src="../assets/nav/technical-docs.svg" alt="Technical documentation"></a>
  <a href="architecture.md"><img src="../assets/nav/architecture.svg" alt="Architecture"></a>
  <a href="event-provenance.md"><img src="../assets/nav/event-provenance.svg" alt="Event provenance"></a>
  <a href="validation.md"><img src="../assets/nav/validation.svg" alt="Validation"></a>
  <a href="data-handling.md"><img src="../assets/nav/data-handling.svg" alt="Data handling"></a>
</p>

<p align="center">
  <a href="how-it-works.md"><img src="../assets/nav/lang-en-selected.svg" alt="English"></a>
  <a href="how-it-works.es.md"><img src="../assets/nav/lang-es.svg" alt="Español"></a>
</p>

<p align="center"><strong>From an OTDR file to a reviewable interpretation without losing data origins.</strong></p>

> **Evidence scope.** This walkthrough explains the behavior documented for **v0.4.5, consolidated on 5 November 2026**. It is not an installation tutorial: the viewer's source code and executable are not publicly distributed. Exact UI control names may vary across revisions, and this page uses named controls only where the record identifies them. The illustrations are **original conceptual diagrams**, not application screenshots or field measurements.

<p align="center">
  <img src="../assets/workflow/flow-light.svg#gh-light-mode-only" alt="Conceptual five-stage OTDR Inside walkthrough, light theme" width="100%">
  <img src="../assets/workflow/flow-dark.svg#gh-dark-mode-only" alt="Conceptual five-stage OTDR Inside walkthrough, dark theme" width="100%">
</p>

## Before you begin · sources and acquisition conditions

Start from files with known acquisition provenance. Compatibility is limited to **characterized formats and layouts**, not universal support by extension.

| Source | Representative path | Important limit |
|---|---|---|
| **EXFO SOR** | Open a trace, inspect native events and use it as a documentary reference. | Choosing a reference does not establish physical ground truth. |
| **Ceyear CE6422 SOR + EI** | Open SOR, then attach its EI only if acquisition and content verification succeed. | Never infer an EI/SOR pair from similar filenames alone. |
| **Yokogawa SOR / SOZ** | Inspect SOR events or select a 1310/1550-nm subtrace in a SOZ container. | Instrument-declared identity can differ from externally confirmed source provenance. |
| **JDSU MSOR** | Open a characterized container and inspect each wavelength with its native events. | The observed MSOR layout is not universal JDSU format support. |

Before comparing, check **wavelength, pulse width, range, group index (IOR), distance origin and acquisition relationship**. A SOZ and its SOR extract may describe the *same* acquisition, not independent repetitions.

## 1. Open and organize measurements

**Operator action.** Open one or more files in characterized formats. The v0.4 workspace can hold up to **eight independent analyses in tabs**. A multi-trace MSOR or SOZ offers wavelength selection while keeping the container as source.

**What to observe.** Each source exposes its recoverable metadata, interpretation state and trace when supported. Tabs preserve independent zoom, selected event, annotations and review.

**EI/SOR-specific route.** From the SOR tab, use the documented **“Associate EI to this SOR”** operation, subject to content/metadata verification. Opening an EI independently does not automatically establish it as the verified companion.

**Engineering decision.** An unreadable or uncharacterized format does not justify guessing units, positions or events. The local workflow is read-only and leaves source measurements unchanged.

## 2. Interpret the trace and events

**Operator action.** Select a tab/subtrace, then inspect the characterized profile, acquisition conditions, reconstructed curve and available event records.

**Keep these categories distinct.**

| Item | Interpretation |
|---|---|
| **SOR/MSOR/SOZ native event** | Instrument-stored record; preserve its class, position and characterized optical fields. |
| **EI imported record** | Record obtained from the observed EI layout; verification of its SOR relationship is a separate property. |
| **Calculated candidate** | Signal-analysis hypothesis with evidence, class and method; not an instrument-stored event. |
| **Suppressed proposal** | Excluded candidate with an inspectable reason; not automatically a confirmed event or match. |
| **Human review** | Added state or annotation; does not change the item's historical source. |

The curve level is **relative**, not universally calibrated optical power. No `KeyEvents` table in a SOR means only that the table was absent; it does not prove that no physical events exist. Reflective/non-reflective terminal candidates require evidence review, not automatic certification as physical fiber ends.

**Stage outcome.** An interpreted trace with clear boundaries among what is *stored*, *imported*, *calculated*, *suppressed* and *pending review*.

## 3. Review evidence, losses and ML suggestions

**Operator action.** Select a relevant event and inspect its native quantities, local evidence and additional estimates where supported.

**Local LSA loss (v0.4.5).** For eligible non-reflective transitions, the viewer estimates loss using local before/after fits and **six window combinations**. Review support, sensitivity and disagreement with instrument-native loss. *Calculated LSA loss* and *stored loss* remain separate. A flagged estimate is not a certified loss value.

**Random Forest morphology advisory.** The active model describes local signal-shape patterns to help prioritize review. Its output is interpreted alongside window evidence and sign/shape disagreements. It **does not** overwrite native classes or losses, and its score is not a calibrated probability.

**Localization and terminals.** The 0.4.2–0.4.5 line preserves baseline positions and records optional experimental edge methods. A ±10% minimum terminal-noise threshold diagnostic warns of sensitivity; it is not metrological uncertainty. Better agreement with one reference is insufficient reason to auto-accept an experimental position.

**Engineering decision.** Keep origins and warnings visible; request review when local windows, native readings and ML suggestions disagree.

## 4. Compare across files and instruments

**Operator action.** Select **two to four previously opened traces**. Check comparability first: wavelength, pulse, IOR, origin, range and whether they describe the same link or acquisition.

**v0.4.5 comparison scope.** Curve overlays, acquisition conditions, event positions, reviewable one-to-one association proposals, unmatched records and **inter-event separation differences**. The operator can choose a comparison reference and inspect original IOR coordinates or a conditional re-expression using a valid reference index.

**How to interpret matching.** EXFO and Yokogawa act as **peer documentary references** in the studied setup; JDSU offers additional corroboration. Selecting any reference does not make it physical ground truth. The v0.4.4-line proximity search of up to **50 m** is a human review aid, not an instrument tolerance. The **15 m** threshold documented for v0.4.1 is a historical, earlier grouping rule, not a substitute for the later workflow.

**Avoid.** Counting a same-acquisition SOR/SOZ as two independent measurements; treating different wavelengths as interchangeable acquisitions; applying IOR conversion without valid inputs; interpreting smaller position differences as demonstrated physical accuracy.

## 5. Save context and generate a report

**Operator action.** Record review decisions and the comparison setup. Where applicable, produce documented outputs (PDF reports, CSV/JSON exports) and retain session state.

**What to preserve.** Version and localization method, source/container/subtrace, acquisition metadata, selected reference and axis, native versus calculated quantities, warnings, association proposals and human decisions. Original measurement files are not overwritten.

**Limits.** IEC-oriented report fields support documentation, **not** IEC certification. Operational reports and exports are not authorized for publication in this public repository.

## Three representative workflows

| Engineering need | Route | Question it helps answer |
|---|---|---|
| **Ceyear SOR + EI** | Open SOR → verify EI association → inspect event/terminal candidates → review LSA and RF context. | What did the algorithm propose, what does EI record and what still requires review? |
| **Yokogawa SOZ** | Open container → select 1310 or 1550 nm → inspect native events → relate a verified SOR extraction. | What belongs to each subtrace and which sources duplicate one acquisition? |
| **Cross-device comparison with JDSU MSOR** | Open MSOR → choose subtrace → select a compatible second source → review associations, IOR and separations. | Which relationships are supportable and which depend on different acquisition conditions? |

## Demonstration is not validation

This page is **not a test session executed today**. It summarizes workflows and decisions reported in development snapshots. Retrospective evidence and its limits are in [Validation](validation.md); release history in [Version history](version-history.md); and source boundaries in [Event provenance](event-provenance.md).

**Actual viewer screenshots are not yet included.** They require a reviewed, permitted capture session following [Data handling](data-handling.md). We do not replace them with external images misrepresented as OTDR Inside screens.

**Not part of packaged v0.4.5:** the isolated 6 November vertical-centering/IOR warning patch, direct EXFO `.stdr` ingestion research and additional model studies. These should not appear in a walkthrough as shipped features.

---

<p align="center">
  <a href="README.md"><img src="../assets/nav/technical-docs.svg" alt="Technical docs"></a>
  <a href="architecture.md"><img src="../assets/nav/architecture.svg" alt="Architecture"></a>
  <a href="data-handling.md"><img src="../assets/nav/data-handling.svg" alt="Data handling"></a>
</p>
