<h1 align="center">OTDR Inside · Technical documentation</h1>

<p align="center">
  <a href="../README.md"><img src="../assets/nav/project-home.svg" alt="Project home"></a>
  <a href="how-it-works.md"><img src="../assets/nav/how-it-works.svg" alt="Guided walkthrough"></a>
  <a href="compatibility-matrix.md"><img src="../assets/nav/compatibility.svg" alt="Compatibility matrix"></a>
  <a href="version-history.md"><img src="../assets/nav/version-history.svg" alt="Version history"></a>
  <a href="architecture.md"><img src="../assets/nav/architecture.svg" alt="Architecture"></a>
  <a href="event-provenance.md"><img src="../assets/nav/event-provenance.svg" alt="Event provenance"></a>
  <a href="validation.md"><img src="../assets/nav/validation.svg" alt="Validation"></a>
  <a href="data-handling.md"><img src="../assets/nav/data-handling.svg" alt="Data handling"></a>
</p>

<p align="center">
  <a href="README.md"><img src="../assets/nav/lang-en-selected.svg" alt="English"></a>
  <a href="README.es.md"><img src="../assets/nav/lang-es.svg" alt="Español"></a>
</p>

This directory contains the public technical documentation for OTDR Inside.

The reconstructed record now covers archived snapshots from **v0.1.0 through v0.4.5**. That range is the material currently analyzed for the public repository, not the endpoint of the project. Later releases should extend the record without rewriting what earlier versions actually demonstrated.

The documentation preserves technical evolution. The current publication scope allows reviewed viewer screenshots, conceptual architecture and independent synthetic/generalized diagrams, **not application source code or private operational measurements**.

## Documentation map

| Page | Purpose |
|---|---|
| <a href="how-it-works.md"><img src="../assets/nav/how-it-works.svg" alt="Guided walkthrough"></a> | Five-stage operator journey through characterized inputs, evidence, LSA/ML review, multi-trace comparisons and reports (documented v0.4.5); no public application code or screenshots. |
| <a href="compatibility-matrix.md"><img src="../assets/nav/compatibility.svg" alt="Compatibility matrix"></a> | Vendor/format coverage for EXFO, Ceyear, Yokogawa and JDSU; status per capability, archived evidence, prerequisites and unproven variants. |
| <a href="version-history.md"><img src="../assets/nav/version-history.svg" alt="Version history"></a> | Release-by-release engineering evolution from v0.1.0 through v0.4.5, including Yokogawa/JDSU/SOZ coverage, loss estimation and documented evidence limits. |
| <a href="architecture.md"><img src="../assets/nav/architecture.svg" alt="Architecture"></a> | Architectural evolution through the v0.4.1 workspace baseline, 4.2 audit and 4.3–4.5 vendor, AI and LSA additions. |
| <a href="event-provenance.md"><img src="../assets/nav/event-provenance.svg" alt="Event provenance"></a> | Source-aware event model across SOR, EI, MSOR and SOZ, including RF advisory output, native loss and LSA estimates. |
| <a href="validation.md"><img src="../assets/nav/validation.svg" alt="Validation"></a> | Validation history through v0.4.5, reference disagreements, release testing and limits of AI/LSA evidence. |
| <a href="data-handling.md"><img src="../assets/nav/data-handling.svg" alt="Data handling"></a> | Permission boundary, screenshot and synthetic-illustration review, and restrictions on source code and operational files. |

## Publication principles

- **structure is not semantics**;
- **stored is not calculated**;
- **proximity is not identity**;
- **unknown is not corrupt**;
- claims stay tied to reproducible evidence and to the release that produced them;
- operational measurements are not required in the public repository to explain the architecture.

---

Next public-facing addition: a **reviewed viewer screenshot gallery** based on the documented v0.4.5 workflow, using permission-cleared UI captures and independent synthetic illustrations. Proposals after the 5 November package are not depicted as shipped features.
