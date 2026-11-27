<h1 align="center">Case studies · Engineering decisions</h1>

<p align="center">
  <a href="../README.md"><img src="assets/nav/project-home.svg" alt="Project home"></a>
  <a href="README.md"><img src="assets/nav/technical-docs.svg" alt="Technical documentation"></a>
  <a href="how-it-works.md"><img src="assets/nav/how-it-works.svg" alt="Guided walkthrough"></a>
  <a href="compatibility-matrix.md"><img src="assets/nav/compatibility.svg" alt="Compatibility"></a>
  <img src="assets/nav/case-studies-current.svg" alt="Case studies">
  <a href="validation.md"><img src="assets/nav/validation.svg" alt="Validation"></a>
</p>

<p align="center">
  <a href="case-studies.md"><img src="assets/nav/lang-en-selected.svg" alt="English"></a>
  <a href="case-studies.es.md"><img src="assets/nav/lang-es.svg" alt="Español"></a>
</p>

> **Scope:** through the v0.4.7 review candidate of 19 November 2026. These studies connect retrospective findings already documented in Validation with invented teaching examples. They are not new viewer tests, screenshots, operational traces or individual records.

<p align="center">
  <img src="assets/case-studies/examples-light.svg#gh-light-mode-only" width="100%" alt="Two synthetic examples">
  <img src="assets/case-studies/examples-dark.svg#gh-dark-mode-only" width="100%" alt="Two synthetic examples">
</p>

## Case A · Why do event positions disagree?

### Problem and engineering decision

Two OTDRs may locate a transition at different positions because of distance origin, group index, pulse width, sampling or localization criteria. Proposed matches do not establish physical identity.

1. Check link, wavelength, pulse and acquisition relationship; do not count a SOZ-derived SOR as an independent acquisition.
2. Retain original positions and provenance (native, EI or calculated).
3. Check IOR and origin; conditionally re-express axes where indices are valid without overwriting originals.
4. Review one-to-one matches, unmatched records, absolute position differences and inter-event separations.
5. Preserve reference disagreements and adverse cases even when a mean improves.

EXFO and Yokogawa serve as peer documentary references in the studied setup; JDSU supplies corroboration and EI supplies records for its Ceyear acquisition. No reference is automatically calibrated physical ground truth.

### Synthetic example · common displacement

Assume two events are already matched and coordinates use comparable axes:

| Quantity | Reference A | Trace B | B − A |
|---|---:|---:|---:|
| Event 1 | 1000 m | 1006 m | +6 m |
| Event 2 | 4000 m | 4006 m | +6 m |
| Separation 2 − 1 | 3000 m | 3000 m | 0 m |

Separation cancels a common additive displacement in this example, but does not establish its origin or physical accuracy.

For the same round-trip delay, the ideal relation is d = c·t/(2·n_g). Re-expression to a valid reference index gives d_ref = d_original·n_original/n_ref, provided origin and time interpretation are compatible. This is conceptual physics, not disclosure of implementation; it does not determine the true index.

### Retrospective evidence · v0.4.4

| Reference (72 event comparisons) | Baseline mean absolute difference | Adaptive edge |
|---|---:|---:|
| EXFO | 4.55 m | 2.88 m |
| JDSU | 4.54 m | 3.88 m |
| Paired EI | 3.78 m | 5.57 m |

Fourteen EXFO comparisons worsened. An alternative two-reference position selector was also evaluated in 4.5 but not activated because separation agreement and some worst cases deteriorated.

**Conclusion:** an improved mean is insufficient to establish a universally better method. These are retrospective agreements for the studied setup, not errors against traceable distances or 72 independent links.

## Case B · What if LSA and native loss disagree?

### Problem and engineering decision

Near non-reflective transitions, windows affected by noise, recovery or another transition may alter loss estimates. An ML morphology suggestion is not a measurement.

The 4.5 line retains native loss and adds local least-squares approximation (LSA) for eligible transitions: conceptually, fit levels on both sides and compare at a common position. Six window combinations expose sensitivity and support; they are neither independent measurements nor a calibrated uncertainty interval. Random Forest advisory output helps review shape and context without overwriting native fields.

### Synthetic example · window sensitivity

Invented values below are not viewer results, internal decision rules or tolerance specifications.

| Variant | Closely grouped fits | Sensitive fits |
|---|---:|---:|
| 1 | 0.19 dB | 0.06 dB |
| 2 | 0.20 dB | 0.10 dB |
| 3 | 0.21 dB | 0.23 dB |
| 4 | 0.20 dB | 0.31 dB |
| 5 | 0.18 dB | 0.44 dB |
| 6 | 0.22 dB | 0.15 dB |
| Max − min | 0.04 dB | 0.38 dB |

With a hypothetical native loss of 0.20 dB, the first case shows close estimates; the second warrants reviewing window selection and signal shape. Neither low spread nor agreement with native loss guarantees accuracy.

### Retrospective evidence · v0.4.5

Of 54 local Ceyear loss estimates, 49 were supported and 5 required review.

| Documentary reference | Mean absolute difference, 1310 nm | 1550 nm |
|---|---:|---:|
| Grouped EXFO | 0.0087 dB | 0.0202 dB |
| Yokogawa, covered regions | 0.0025 dB | 0.0064 dB |

Coverage differs and records depend on the studied setup. These are not calibrated uncertainties or proof of vendor superiority. In another batch, numerical review flagged 75 items and joint ML/context review flagged 167, including 92 additional items; these are review counts, not classifier precision or recall.

**Conclusion:** retaining native loss, LSA, local evidence and review separately makes discrepancies auditable without automatic corrections based on RF scores.

## Case C · Legacy formats and non-equivalent measurements

A synthetically illustrated legacy SOR may place a launch connector at a negative native coordinate while the plotted curve starts at zero. Preserve both axes rather than silently shifting the native table. In iOLM, a report trace may accompany a consolidated multi-pulse event table; a grouped element is not another independent event. For example, a hypothetical loss reported as > 5 dB is a lower bound, not an exact 5 dB observation. The observed legacy and iOLM reader revisions retain these distinctions. NOVKER's provisional fields remain explicitly tentative. Same-name files that disagree in samples or acquisition cannot be merged.

## Case D · Why a position model should be able to abstain

Suppose an eligible event has three illustrative coordinates, 1000, 1002 and 1004 m. These are fictional proposals, not measured results. If the local curve does not support a unique association or proposals conflict with neighboring events, the appropriate decision may be no proposal, not automatic averaging. The 4.7 candidate compares baseline, edge and RF position with a Sugeno advisory decision. This is distinct from morphology classification. Archived cross-reference studies showed reference-dependent outcomes, and not all eligible events had a proposal. Retain native coordinates and the reason for abstention; verify physical accuracy independently.

## Development status

| State at documentation cutoff | Items |
|---|---|
| Included in 4.5 | Characterized SOR/EI/MSOR/SOZ, reviewable matching, conditional IOR, separations, local LSA, contextual RF advice and reports. |
| Experimental in 4.x | Alternative localization with reference-dependent results. |
| Evaluated but not activated | Two-reference position selector and external model studies. |
| Included in 4.6 | Revised centering, review queue, conditional matching and expanded readers. |
| Review candidate 4.7 | Optional position pilot, magnitude-specific eligibility and editable measurement context. |
| Pending in reviewed record | Direct STDR ingestion, independent new-link references and inspection of final Windows build. |

The 19 November candidate cutoff does not claim inspection of the final Windows release. See [Validation](validation.md) and [Version history](version-history.md) for archived results and [Data handling](data-handling.md) for publication boundaries.

---

<p align="center">
  <a href="README.md"><img src="assets/nav/technical-docs.svg" alt="Technical documentation"></a>
</p>
