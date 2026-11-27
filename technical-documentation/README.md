<h1 align="center">OTDR Inside · Technical documentation</h1>

<p align="center">
  <a href="../README.md"><img src="assets/nav/project-home.svg" alt="Project home"></a>
  <a href="how-it-works.md"><img src="assets/nav/how-it-works.svg" alt="Guided walkthrough"></a>
  <a href="compatibility-matrix.md"><img src="assets/nav/compatibility.svg" alt="Compatibility matrix"></a>
  <a href="case-studies.md"><img src="assets/nav/case-studies.svg" alt="Case studies"></a>
  <a href="version-history.md"><img src="assets/nav/version-history.svg" alt="Version history"></a>
  <a href="architecture.md"><img src="assets/nav/architecture.svg" alt="Architecture"></a>
  <a href="event-provenance.md"><img src="assets/nav/event-provenance.svg" alt="Event provenance"></a>
  <a href="validation.md"><img src="assets/nav/validation.svg" alt="Validation"></a>
  <a href="data-handling.md"><img src="assets/nav/data-handling.svg" alt="Data handling"></a>
</p>

<p align="center">
  <a href="README.md"><img src="assets/nav/lang-en-selected.svg" alt="English"></a>
  <a href="README.es.md"><img src="assets/nav/lang-es.svg" alt="Español"></a>
</p>

This directory contains the public technical documentation for OTDR Inside.

The reconstructed record covers v0.1.0 through the **v0.4.7 review candidate of 19 November 2026**, including intermediate 4.6 revisions. Documented through the **v0.4.7 review candidate (19 November 2026)**. The later standalone Windows build and its final adjustments are not yet independently inspected here.

The documentation preserves technical evolution. The current publication scope allows reviewed viewer screenshots, conceptual architecture and independent synthetic/generalized diagrams, **not application source code or private operational measurements**.

## Documentation map

| Page | Purpose |
|---|---|
| <a href="how-it-works.md"><img src="assets/nav/how-it-works.svg" alt="Guided walkthrough"></a> | Five-stage operator journey through characterized inputs, evidence, LSA/ML review, multi-trace comparisons and reports (v0.4.7 review candidate); no public application code or screenshots. |
| <a href="compatibility-matrix.md"><img src="assets/nav/compatibility.svg" alt="Compatibility matrix"></a> | Vendor/format coverage for EXFO, Ceyear, Yokogawa, JDSU, NOVKER and structural F2H; status per capability, archived evidence, prerequisites and unproven variants. |
| <a href="case-studies.md"><img src="assets/nav/case-studies.svg" alt="Case studies"></a> | Position agreement, LSA loss, legacy readers and advisory localization with synthetic examples, archived evidence and development status. |
| <a href="version-history.md"><img src="assets/nav/version-history.svg" alt="Version history"></a> | Release-by-release engineering evolution from v0.1.0 through v0.4.7 candidate, including Yokogawa/JDSU/SOZ coverage, loss estimation and documented evidence limits. |
| <a href="architecture.md"><img src="assets/nav/architecture.svg" alt="Architecture"></a> | Architectural evolution through the v0.4.1 workspace baseline, 4.2 audit and 4.3–4.7 reader, ML, review and comparison additions. |
| <a href="event-provenance.md"><img src="assets/nav/event-provenance.svg" alt="Event provenance"></a> | Source-aware event model across characterized inputs, native and calculated positions, including RF advisory output, native loss and LSA estimates. |
| <a href="validation.md"><img src="assets/nav/validation.svg" alt="Validation"></a> | Validation history through the v0.4.7 candidate, reference disagreements, release testing and limits of AI/LSA evidence. |
| <a href="data-handling.md"><img src="assets/nav/data-handling.svg" alt="Data handling"></a> | Permission boundary, screenshot and synthetic-illustration review, and restrictions on source code and operational files. |

## Publication principles

- **structure is not semantics**;
- **stored is not calculated**;
- **proximity is not identity**;
- **unknown is not corrupt**;
- claims stay tied to reproducible evidence and to the release that produced them;
- operational measurements are not required in the public repository to explain the architecture.

---

Next: verify the final Windows build and add permission-cleared viewer screenshots. The candidate is not represented as the final executable.
