<h1 align="center">Compatibility matrix · OTDR Inside</h1>

<p align="center">
  <a href="../README.md"><img src="../assets/nav/project-home.svg" alt="Project home"></a>
  <a href="README.md"><img src="../assets/nav/technical-docs.svg" alt="Technical documentation"></a>
  <a href="how-it-works.md"><img src="../assets/nav/how-it-works.svg" alt="Guided walkthrough"></a>
  <img src="../assets/nav/compatibility-current.svg" alt="Compatibility matrix · current page">
  <a href="architecture.md"><img src="../assets/nav/architecture.svg" alt="Architecture"></a>
  <a href="validation.md"><img src="../assets/nav/validation.svg" alt="Validation"></a>
  <a href="data-handling.md"><img src="../assets/nav/data-handling.svg" alt="Data handling"></a>
</p>

<p align="center">
  <a href="compatibility-matrix.md"><img src="../assets/nav/lang-en-selected.svg" alt="English"></a>
  <a href="compatibility-matrix.es.md"><img src="../assets/nav/lang-es.svg" alt="Español"></a>
</p>

<p align="center"><strong>Opening a file does not mean that every event, optical quantity or comparison has been validated.</strong></p>

> **Documentation cutoff: v0.4.5, consolidated on 5 November 2026.** This matrix summarizes capabilities recorded in archived development packages and reports. It is **not** a newly executed test, Telcordia/IEC conformity certification or blanket support promise for all equipment from a vendor. Source code and raw verification corpora remain private under the [data handling policy](data-handling.md).

<p align="center">
  <img src="../assets/compatibility/coverage-light.svg#gh-light-mode-only" alt="Conceptual source routes for SOR, EI, MSOR and SOZ, light theme" width="100%">
  <img src="../assets/compatibility/coverage-dark.svg#gh-dark-mode-only" alt="Conceptual source routes for SOR, EI, MSOR and SOZ, dark theme" width="100%">
</p>

## Status terminology

| Status | Technical meaning |
|---|---|
| **Characterized** | Relevant structure and semantics were identified for the stated layout and archived regression evidence exists. Does not imply universal coverage or calibrated accuracy. |
| **Conditional** | The function is supported only for specific profiles, event classes or verified relationships and requires stated preconditions. |
| **Structural only** | Recoverable structure may be inspected without assigning optical meaning to every field. |
| **Not demonstrated** | The available record does not justify claiming this capability; this does **not** mean it is impossible. |
| **Research / not packaged** | Experimental or proposed work is not part of the original packaged v0.4.5 delivery. |

Statuses apply **per capability**, not to an entire brand. Equipment identity declared in a file, evidence-backed source attribution and the software that exported it are distinct concepts.

## 1. Vendor and format matrix

| Ecosystem and format | Documented coverage through 4.5 | Events and computed results | Key boundary | Since |
|---|---|---|---|---|
| **EXFO · SOR 2.00** | **Characterized:** FTB-7200 reference profile, structural parsing, normalized parameters and trace reconstruction. | **Characterized:** `KeyEvents` when present; positions and fields supported by the profile. | **Documentary reference**, not an infallible physical ground truth or all-EXFO compatibility. | 0.1.0 |
| **Ceyear CE6422 · SOR** | **Characterized:** trace and parameters for the observed layout, event hypotheses and terminal evidence. | **Conditional:** calculated events, suppressed proposals, RF review and local LSA for eligible transitions. A SOR lacking `KeyEvents` has no verified stored-event table. | Detector results do not automatically certify physical positions/losses. LSA/RF cannot overwrite native readings. | 0.2.0 / 0.3.x / 0.4.5 |
| **Ceyear CE6422 · EI** | **Characterized for observed binary layout:** defensive import and verified acquisition/content relationship to SOR. | **Conditional:** EI-imported records retain distinct origins and may be compared to calculated candidates. | Not universal EI support or filename-based pairing. EI is not reclassified as SOR `KeyEvents`. | 0.3.3 |
| **Yokogawa · SOR with declared identity** | **Characterized:** native event interpretation for original files declaring AQ1000. | **Characterized:** original `KeyEvents` and studied optical fields, preserving stored metadata. | Identification applies to the observed files and not every Yokogawa variant. | 0.4.3 |
| **Yokogawa · SOR with externally confirmed provenance** | **Characterized for specific originals:** source attribution confirmed for that set although their metadata do not declare the model. | **Characterized in that set:** native records recovered without rewriting original fields. | Confirmation must not be presented as instrument-declared metadata or generalized from software signatures. | 0.4.3 |
| **Yokogawa · SOZ** | **Characterized:** observed multitrace container with 1310/1550-nm members, samples and per-subtrace metadata. | **Characterized:** native event records per member; **conditional:** same-acquisition SOR extract association. | A SOZ and its SOR copy are **not independent acquisitions**; other SOZ variants remain unproven. | 0.4.5 |
| **JDSU T-BERD 6000A / 8126 · MSOR** | **Characterized:** observed **VSRE 2.10** two-wavelength container, sample data and member context. | **Characterized where evidence supports:** native events and recognized fields; **conditional:** verified source-specific external TXT pairing. | Do not extend units, field semantics or compatibility to unobserved JDSU/MSOR layouts. | 0.4.4 |
| **Uncharacterized vendor SOR** | **Structural only, if accepted by the reader:** inspect recognizable SOR blocks. | **Not demonstrated** for vendor-specific event scales and optical magnitudes. | Opening a file does **not** establish correct optical interpretation or universal SOR coverage. | Varies |

**Yokogawa identity note:** the v0.4.3 archive distinguishes **24 files with declared identity** and **24 whose source was confirmed only for those originals**. The aggregate verification covers 48 files and 125 native records. These figures do not predict future support for all Yokogawa files.

## 2. Cross-format capabilities and prerequisites

| Capability | Documented state through v0.4.5 | Preconditions and interpretation |
|---|---|---|
| **Structure and trace reconstruction** | Characterized for the listed profiles. | Block/sample layout, units and distance axis must match the observed design. |
| **Native events and stored optical fields** | Characterized **where a supported event table/field exists**. | Preserve positions, classes, raw values and source; do not fabricate unavailable stored values. |
| **Calculated events and terminal hypotheses** | Conditional on reconstructed trace and relevant detector path. | Preserve localization method, evidence, warnings and suppressions. Not automatically certified physical endpoints. |
| **Ceyear EI/SOR association** | Conditional on a verified pair. | Compare content/acquisition metadata, keep EI and SOR origins distinct. |
| **MSOR/SOZ multi-wavelength containers** | Characterized for the observed JDSU/Yokogawa layouts. | Subtraces carry individual wavelengths, samples, events and origins. |
| **SOZ/SOR extraction association** | Conditional on demonstrable same-acquisition relationship. | Do not double-count one acquisition as independent samples. |
| **Multi-file workspace** | Documented: up to **8 analysis tabs**, **2–4 traces per comparison**. | Review/session state remains isolated per source or member trace. |
| **Cross-file event matching** | Reviewable one-to-one proposals and unmatched records. | Up to **50 m** in the characterized v0.4.4-line review search is not a physical tolerance; the **15 m v0.4.1** rule belongs to earlier grouping logic. |
| **Reference and group index (IOR)** | Reference selection and **conditional** coordinate re-expression where IOR is valid. | Preserve source axes; missing IOR prevents silent conversion. Re-expression is not calibration. |
| **Inter-event separations** | Derived differences included in v0.4.5 comparisons. | Intervals share endpoints and are not independent observations. |
| **Instrument-native event loss** | Available only where the profile supports that field's semantics. | Keep native loss distinct from estimates; vendors need not share algorithms. |
| **Local LSA event loss** | **Conditional:** eligible non-reflective changes and six before/after window variants. | Store estimates, support, sensitivity and disagreements, not calibrated uncertainty. |
| **Random Forest morphology** | **Contextual advisory**, not an instrument reading. | Active weights unchanged in v0.4.5; scores are not calibrated probabilities and do not rewrite native data. |
| **Sessions and reports** | Documented with method, source/container/member, reference/axis and review; PDF/CSV/JSON according to workflow. | Local outputs; operational reports and exports are not public data. |

## 3. Evidence behind the matrix

These are **archived results**, not tests run from this GitHub repository.

| Milestone | Recorded evidence | Supported conclusion |
|---|---|---|
| **Early EXFO profile** | Characterized reader/profile and structural/trace regression across the 0.1 line. | Studied profile can act as a documentary reference. |
| **Ceyear EI/SOR · 0.3.6** | 18 controlled pairs and **72/72** post-start event associations. | Consistency under those study conditions, not absolute accuracy or independent links. |
| **Yokogawa SOR · 0.4.3** | **48 files**, **125 stored events**, with declared and confirmed-only identity distinguished. | Observed-lot coverage, not universal vendor compatibility. |
| **JDSU MSOR · 0.4.4** | Dual-wavelength container inspection and source-specific reference comparisons. | Studied layout characterized, not generic MSOR support. |
| **Yokogawa SOZ · 0.4.5** | **9 containers → 18 wavelength members → 69 stored events**. | Preservation of observed structure, wavelength members and native records. |
| **Ceyear LSA · 0.4.5** | **54 estimates**, **49 supported and 5 requiring review**. | Retrospective agreement and contextual review, not calibrated loss accuracy. |
| **Consolidation · 0.4.5** | **123 Python tests** reported with available internal corpora plus simulated DOM coverage. | Development regression, **not** a public executable test suite or a completed real Windows-browser test. |

For further detail, see [Validation](validation.md), [Version history](version-history.md) and [Architecture](architecture.md).

## 4. Checklist before comparing two files

1. **Identify the actual source:** separate declared instrument/model, exporter signature and source attribution confirmed for specific originals.
2. **Confirm format family:** do not extrapolate a SOR, EI, MSOR or SOZ profile to an uncharacterized variant.
3. **Review acquisition conditions:** wavelength, pulse, range, resolution, distance origin and available IOR.
4. **Verify relationships:** EI/SOR pairs or same-acquisition SOZ/SOR copies; do not count duplicates as independent trials.
5. **Separate evidence origins:** native events, imported EI, calculated candidates, LSA loss and RF advice are not interchangeable.
6. **Audit comparison choices:** reference, original/transformed axis, unreviewed matching proposals and window/loss warnings.

**Decision rule:** when a prerequisite is missing, show only what is recoverable and mark the comparison or measurement **not demonstrated**, rather than inventing certainty.

## 5. Not included in the packaged coverage

| Item | Documented status |
|---|---|
| **Direct EXFO `.stdr`** | Research as of 6 November; **not packaged** in v0.4.5. |
| **TRC and uncharacterized EI/MSOR/SOZ variants** | **Support not demonstrated** in the normal v0.4.5 workflow. |
| **6 November vertical-centering / IOR-warning patch** | Isolated experiment; **not merged** into the original 4.5 ZIP. |
| **Additional ML models and alternative RF position selector** | External/retrospective experiments; did not replace the shipped Random Forest or position rules. |
| **Universal compatibility, Telcordia/IEC certification and calibration** | **Not claimed**. Independent evidence and appropriate normative scope remain unavailable. |

## Updating this matrix

Every later version should record **model/layout signature**, **format and variant**, **exact capabilities**, **ground-truth/reference basis**, **test corpus aggregates without private IDs**, **limitations** and the **version that introduced the behavior**. Adding a vendor name without that evidence is insufficient.

This is an **engineering evidence-based coverage matrix**, not a commercial list of “certified compatible” devices.

---

<p align="center">
  <a href="how-it-works.md"><img src="../assets/nav/how-it-works.svg" alt="Guided walkthrough"></a>
  <a href="validation.md"><img src="../assets/nav/validation.svg" alt="Validation"></a>
  <a href="README.md"><img src="../assets/nav/technical-docs.svg" alt="Technical documentation"></a>
</p>
