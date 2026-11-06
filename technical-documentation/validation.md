<h1 align="center">Validation · evidence hierarchy and documented limits</h1>

<p align="center">
  <a href="../README.md"><img src="../assets/nav/project-home.svg" alt="Project home"></a>
  <a href="README.md"><img src="../assets/nav/technical-docs.svg" alt="Technical docs"></a>
  <a href="architecture.md"><img src="../assets/nav/architecture.svg" alt="Architecture"></a>
  <a href="event-provenance.md"><img src="../assets/nav/event-provenance.svg" alt="Event provenance"></a>
  <img src="../assets/nav/validation-current.svg" alt="Validation · current page">
  <a href="data-handling.md"><img src="../assets/nav/data-handling.svg" alt="Data handling"></a>
</p>

<p align="center">
  <a href="validation.md"><img src="../assets/nav/lang-en-selected.svg" alt="English"></a>
  <a href="validation.es.md"><img src="../assets/nav/lang-es.svg" alt="Español"></a>
</p>

<p align="center"><strong>Validation is layered evidence, not a single accuracy score.</strong></p>

> **Scope.** This page summarizes the validation model documented by archived releases through **v0.4.5**. Operational traces and private corpora remain outside the public repository, so this page reports methods and aggregate results without publishing route names, customer identifiers, private filenames or hashes.

<p align="center">
  <img src="../assets/validation/layers-light.svg#gh-light-mode-only" alt="OTDR Inside validation hierarchy in light mode" width="100%">
  <img src="../assets/validation/layers-dark.svg#gh-dark-mode-only" alt="OTDR Inside validation hierarchy in dark mode" width="100%">
</p>

## Validation layers

OTDR Inside does not use one pass/fail metric for everything. Different claims require different evidence.

| Layer | What is checked | What it can support |
|---|---|---|
| **Structural safety** | Block bounds, revisions, sizes, paths and invalid input handling. | The file can be inspected without treating malformed structure as valid data. |
| **Semantic profile regression** | Known profile rules, raw/normalized values and unsupported states. | A particular field meaning is supported for a characterized profile. |
| **Trace reconstruction** | Sample counts, distance axis, relative level and deterministic reconstruction. | The displayed curve is reproducible under the documented interpretation. |
| **Event / terminal regression** | Candidate positions/classes, suppressed reasons, terminal logic and deterministic output. | The algorithm behaves consistently for the tested cases. |
| **Paired-format consistency** | Verified EI/SOR sample relation, metadata and event association. | Two related formats are handled consistently under the characterized family. |
| **Workspace / comparison regression** | Tabs, isolated session state, EI association, comparison grouping and drawing rules. | Multi-file interaction preserves per-file results and follows documented presentation logic. |
| **Manual / reference review** | Browser rendering, vendor/reference software comparison and targeted field inspection. | Interface and interpretation can be checked against external observations, with limits kept explicit. |

No single layer turns the project into a calibrated OTDR measurement standard.

## Public evidence vs. private evidence

| Evidence type | Public repository | Purpose |
|---|---|---|
| Conceptual diagrams and independently synthetic illustrations | **Allowed** | Explain architecture, event concepts and interface behavior without publishing the implementation. |
| Screenshots of the real viewer | **Allowed after per-image review** | Demonstrate an actual version only after removing identifying metadata and operational measurement content. |
| Aggregated validation methods and outcomes | **Documentable after review** | Describe tested scope, reference assumptions, limitations and unresolved questions. |
| Source code, internal tests, scripts, proprietary profiles and build files | **Not authorized for publication** | The implementation stays outside GitHub even if renamed or sanitized. |
| Operational SOR/EI, samples, events, session exports, derived traces and customer data | **Private** | Field/reference validation remains in the authorized work environment. |
| Vendor executables, commercial manuals and licensed standards | **Not redistributed** | Cite or link lawfully available sources where helpful. |

The documented test counts are results from archived development reports. **No public source-code migration or runnable public CI suite is currently planned under this permission scope.** See the <a href="data-handling.md">data handling and publication policy</a>.

## Release evidence

### v0.1.0 · baseline automation

The first archived vertical slice records **23 automated tests passing** around the parser, profiles, trace and viewer baseline.

### v0.3.2 · detector-development checkpoint

The archived 0.3.2 material records **59 tests** with controlled Ceyear and EXFO material. It also reports an improved development comparison for intermediate event references while explicitly stating those references were **not blind evaluation data**.

That limitation matters: development agreement is evidence of progress, not an independent accuracy estimate.

### v0.3.4 and v0.3.5 · terminal regression grows

The archived result sets record **71 tests** for v0.3.4 and **76 tests** for v0.3.5 as terminal diagnostics and evidence logic were added. The count increase reflects broader regression coverage, not a directly comparable quality score.

### v0.3.6 · paired-format consistency

The v0.3.6 release validation records:

| Check | Documented result |
|---|---|
| Release validation | **80 tests passed** |
| Controlled EI/SOR pairs | **18** |
| Useful complementary samples checked | 18 × 16,384 |
| Post-start EI records associated with calculated candidates | **72/72** |
| Extra calculated candidates in that controlled pair corpus | **0** |
| Candidate indices/classes across paired EI/SOR analyses | Matched in all 18 pairs |
| Private field SOR traces examined | **27** |
| Field traces with unchanged candidate indices/classes vs. v0.3.5 | **22** |
| Field traces with changes requiring review | **5** |

The archived report also records a mean absolute SOR-versus-EI position difference of **3.782 m** and a maximum of **17.154 m** in the controlled pair set. Those figures are described as concordance under the studied conditions, **not OTDR distance accuracy or uncertainty**.

The release specifically improved one weak-response localization case, but the report labels that adjustment retrospective rather than independent validation.

### v0.4.0 · workspace plus stricter terminal competition

v0.4.0 re-ran the detector over:

- **27 private field SOR traces**;
- **18 controlled EI/SOR pairs** spanning the documented acquisition combinations.

The controlled pairs preserved **72/72 associations**, with no additional unmatched calculated candidates. Relative to v0.3.6, only one private field trace changed terminal class under the stricter reflective-end competition rule.

The important conclusion is narrow: the rule changed the intended weak terminal case while preserving the controlled pair behavior. It does not certify a physical endpoint.

### v0.4.1 · comparator regression

v0.4.1 intentionally keeps the detector from v0.4.0 and validates the corrected comparison layer.

The archived validation records:

- **54 Python tests passed**;
- **6 external-corpus tests skipped** in standard discovery;
- simulated production JavaScript with DOM/canvas coverage;
- all-pairs event grouping for the tested comparison;
- common-span/full-span switching;
- the 5–50 m median baseline for the tested outlier case;
- preservation of per-tab state and explicit EI association.

At the time of the archived validation, a final visual inspection of the revised comparator in a real Windows browser was still pending. That pending check is part of the record rather than being silently converted into a completed validation claim.

### v0.4.2 · visible evidence, experimental edges and reporting

The v0.4.2 archive describes multiple revisions under the **same** version number.

| Documented stage | Evidence |
|---|---|
| Initial comparison audit (29 September) | 55 internal tests passed; 6 external-corpus tests skipped. Comparison distinguishes visible from suppressed proposals, and terminal-threshold sensitivity uses ±10% of the minimum-noise threshold. |
| Numerical edge experiment (1 October) | Class-aware method evaluated retrospectively against 18 EI/SOR controlled pairs and EXFO comparison material. Baseline positions remain reproducible and default. |
| Final documented presentation revision (8 October) | **92 Python tests passed without skips with controlled/private corpora available**, plus simulated DOM and session tests. Example single-file and comparison PDFs were rendered and reviewed page-by-page. A live Windows-browser test was not performed in that environment. |

**Contradictory reference agreement is preserved, not averaged away.**

| Reference comparison (same 72 calculated events from 18 controlled EI/SOR pairs) | Baseline mean absolute difference | Experimental edge mean absolute difference |
|---|---:|---:|
| EXFO study comparison | **4.399 m** | **2.726 m** |
| Paired EI measurement | **3.782 m** | **5.532 m** |

The 18 pairs are repeated measurements from a controlled arrangement, **not 18 independent links**; the 72 event positions are not independent link samples. Moreover, the physical identity of the EXFO and Ceyear acquisition path has not been established. These numbers describe **retrospective software/reference concordance, not metrological error**.

On 27 private field SOR traces, the baseline reproduces the preceding event set; the experimental method adjusted 87 of 189 visible calculated positions and kept the other 102. This preserves event IDs/classes and suppressed histories but **does not prove the adjusted positions physically better**. In an exploratory second-link comparison against SOR-stored records, mean absolute difference worsened from 1.233 m to 1.423 m. The class-aware method is therefore **experimental, optional, reversible and disabled by default**.

Terminal minimum-noise threshold perturbation provides sensitivity warnings, **not confidence intervals** or physical-end verification. Nearby suppressed candidates may be shown as audit context but must not be counted as visible event matches.

**Publication boundary.** Only these aggregate methodological results belong in the public documentation. Real traces, per-event records, corpus identifiers, serials, hashes, absolute paths and PDF reports containing operational metadata remain private. The 92-test result is reported from the archived v0.4.2 validation document, **not a public CI run** or a newly executed GitHub test suite.

### v0.4.3 · Yokogawa source characterization and morphology review

The corrected 4.3 release documents **48 original Yokogawa-lot SOR files**: 24 identify the model in instrument metadata, 24 have source provenance confirmed for those exact files while their original manufacturer/model fields remain unchanged. All **125 stored event records** (49 at one wavelength, 76 at the other) were recovered in the archived verification. This is source-specific coverage, not a blanket claim of universal Yokogawa interoperability or certified event-loss accuracy.

The Random Forest morphology assistant is an advisory classifier. The archive does not establish a calibrated probability interpretation for its scores or external physical-truth accuracy.

### v0.4.4 · JDSU MSOR and edge-method tradeoffs

The archived 4.4 material exercises the **observed JDSU dual-wavelength MSOR 2.10** reading, recorded native events and source-constrained TXT references. The revised comparison proposes one-to-one associations and retains unmatched events; the 50 m search range is a **review aid**, not a calibration tolerance.

The evaluated adaptive edge method was tested on **72 controlled event comparisons**. Its mean absolute *reference disagreement* changed **4.55 → 2.88 m** versus EXFO, **4.54 → 3.88 m** versus JDSU and **3.78 → 5.57 m** versus acquisition-paired EI; 14 EXFO comparisons worsened. References share a studied arrangement, with different instrument/pulse settings. These are **not physical distance errors**, and the worsening EI/individual cases prevents a universal accuracy claim.

### v0.4.5 · SOZ coverage, local LSA loss and contextual review

The 5 November consolidated v0.4.5 release documents the following retrospective checks:

| Category | Archived result | What it does **not** establish |
|---|---|---|
| Yokogawa SOZ containers | **9 files → 18 wavelength traces → 69 stored events** preserved | Universal SOZ support or independent acquisitions for duplicate SOR copies. |
| Local LSA splice-loss estimation | **54 estimates**, with **49 supported** and **5 flagged for review** | Calibrated loss uncertainty, universal equivalence to the native instrument algorithm. |
| LSA vs grouped EXFO references | Mean absolute difference **0.0087 dB (1310 nm)** and **0.0202 dB (1550 nm)** for the studied three splice zones | Independent accuracy or statistically independent links. |
| LSA vs characterized Yokogawa references | Mean absolute difference **0.0025 dB (1310 nm)** and **0.0064 dB (1550 nm)** for covered 5/8-km zones | A universal loss tolerance or resolution of all contradictory stored values. |
| Final 5 November regression | **123 Python tests passed**, plus simulated DOM paths with available internal data | Public CI reproducibility, Windows execution or real-browser visual verification. |
| Extracted-package check | **25 targeted tests passed**; manifest/CRC and example PDF rendering inspected | Replacement for the broader suite, or a certification test. |

The consolidation preserves **86 traces, 441 calculated positions, 86 curves and 245 local LSA estimates** before/after the operator-report revisions; source values and active morphology weights were unchanged. Eighteen raw terminal zero-loss values were rendered as **not applicable** in the operator view while retaining their original raw records. This is a presentation/semantics decision, not deletion of the original data.

An active Random Forest morphology model is combined with window stability and native-versus-LSA discrepancies **for review**, not automatic corrections. In one retrospective batch, **75** items triggered numerical window review and **167** triggered the joint model/context review, including 92 not already flagged numerically. These are **workflow counts, not precision or recall**.

The archived alternative reference-trained position selector improved some mean absolute position differences but worsened EXFO inter-event separation agreement and certain worst-case events. It was **not activated** in the packaged viewer.

**Status of 6 November investigations (outside the packaged 4.5).** A separate comparator-centering/IOR-warning patch passed **124 Python tests** with available corpora and simulated JavaScript, but it remained a separate patch, not an update to the original package; direct `.stdr` support was only investigated. The contemporaneous RF-versus-MLP/Extra Trees/HistGradientBoosting study was conducted **outside the viewer** and did not change its active model. These results may be documented as research, but not called deployed 4.5 capabilities.

**Metrological limit.** EXFO and Yokogawa can be treated as peer documentary comparison references in this controlled workflow; selecting one does not make its reading physically exact. JDSU is additional contextual corroboration. Re-expressing an axis to a reference IOR is a conditional coordinate transformation, not determination of the true group index or instrument calibration. IEC-oriented record fields and archived test counts are not certificates. A real Windows/browser run and independent new-link physical labels remain to be demonstrated.

## Why test counts are not a score

Test counts differ because the suites evolved:

- new modules create new tests;
- some releases run external/private corpora unavailable in standard discovery;
- browser or corpus-dependent tests may be conditional;
- one release may emphasize detector regression while another emphasizes workspace behavior.

Therefore, **80 vs. 54 does not mean one version is “more validated” than the other**. The useful question is which claims each suite covers and under which data conditions.

## Comparison-specific limits

The v0.4.1 comparator adds useful inspection tools, but its rules are intentionally non-metrological.

### 15 m proximity threshold

The 15 m grouping threshold is a visual review aid. It is not:

- a calibrated tolerance;
- an event-identity rule;
- a claim of link identity;
- a confidence interval;
- an accuracy specification.

### All-pairs grouping

v0.4.1 improves over v0.4.0 by checking proximity across all selected files instead of only against the first selection. The grouping is still greedy and may not be globally optimal when several events are very close.

### Curve baseline and span

The 5–50 m median baseline, terminal-noise attenuation and common/full-span switch are **drawing rules**. They do not modify source samples, event positions, loss values or provenance.

## What would be needed for physical accuracy claims

A metrological study would require, at minimum:

- fiber/reference points with traceable distance and known uncertainty;
- explicit launch/origin definition;
- verified group index and OTDR acquisition settings;
- repeated acquisitions across wavelength and pulse classes;
- independent event-class labels;
- enough repetitions to estimate variability;
- blind validation material not used to tune the detector.

Until that exists, the project should describe distances as algorithm/reference **concordance** under specific conditions rather than universal OTDR accuracy.

## Rules for public validation documentation

- preserve raw and normalized values separately;
- keep stored/imported/calculated/manual provenance explicit;
- keep source code, private tests and operational regression outside GitHub;
- use independently synthetic visuals and reviewed screenshots; do not upload private or adapted test fixtures;
- document when a test depends on external corpora;
- distinguish automated regression from browser/manual inspection;
- distinguish visual proximity from physical tolerance;
- retain failed, pending and negative evidence instead of reporting only successful cases.

---

<p align="center">
  <a href="README.md"><img src="../assets/nav/technical-docs.svg" alt="Technical docs"></a>
  <a href="event-provenance.md"><img src="../assets/nav/event-provenance.svg" alt="Event provenance"></a>
</p>

<p align="center"><sub>Publication boundary: screenshots and conceptual material are allowed after review; application source code remains excluded.</sub></p>
