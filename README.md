<p align="center">
  <picture>
    <source media="(prefers-color-scheme: dark)" srcset="https://raw.githubusercontent.com/EstebanErazo500/OTDR_Inside/main/assets/otdr-hero.svg">
    <source media="(prefers-color-scheme: light)" srcset="https://raw.githubusercontent.com/EstebanErazo500/OTDR_Inside/main/assets/otdr-hero-light.svg">
    <img src="https://raw.githubusercontent.com/EstebanErazo500/OTDR_Inside/main/assets/otdr-hero-light.svg" alt="OTDR Inside" width="100%">
  </picture>
</p>

<p align="center">
  <strong>Evidence-aware analysis of OTDR measurements.</strong><br>
  Safe SOR parsing, vendor-aware interpretation, trace reconstruction, paired EI/SOR analysis and calculated event evidence without losing data provenance.
</p>

<p align="center">
  <a href="#overview"><img src="assets/nav/overview.svg" alt="Overview"></a>
  <a href="#architecture"><img src="assets/nav/architecture.svg" alt="Architecture"></a>
  <a href="#current-capabilities"><img src="assets/nav/capabilities.svg" alt="Capabilities"></a>
  <a href="#current-scope"><img src="assets/nav/scope.svg" alt="Scope"></a>
  <a href="#validation"><img src="assets/nav/validation.svg" alt="Validation"></a>
  <a href="#roadmap"><img src="assets/nav/roadmap.svg" alt="Roadmap"></a>
  <a href="technical-documentation/README.md"><img src="assets/nav/technical-docs.svg" alt="Technical Docs"></a>
</p>

<p align="center">
  <a href="README.md"><img src="assets/nav/lang-en-selected.svg" alt="English"></a>
  <a href="README.es.md"><img src="assets/nav/lang-es.svg" alt="Español"></a>
</p>

---

<h2 id="overview" align="center">Overview</h2>

OTDR SOR files are designed to store optical time-domain reflectometry measurements, but real-world files are not always semantically uniform. Vendor extensions, rewritten metadata, ambiguous scales and different event representations can make a file structurally readable without making every value equally trustworthy.

OTDR Inside addresses that problem by separating **structure**, **interpretation**, **calculation**, **evidence** and **provenance**. The goal is not to force every trace into a universal model, but to expose what is known, how it was derived and what remains unresolved.

<h2 id="architecture" align="center">Architecture</h2>

<p align="center">
  <img src="assets/architecture-v2.svg" alt="OTDR Inside analysis pipeline" width="94%">
</p>

The pipeline is deliberately layered:

| Layer | Responsibility |
|---|---|
| **Structural parsing** | Reads SOR blocks, revisions, sizes, offsets and sample regions with explicit boundary checks. |
| **Profile & normalization** | Applies vendor-aware rules only when the available evidence supports them. |
| **Trace & event analysis** | Reconstructs the curve, keeps stored/imported/calculated origins separate and evaluates terminal evidence without inventing physical magnitudes. |
| **Presentation** | Exposes parameters, structure, provenance, events and evidence through a local viewer and JSON/CSV exports. |

This separation prevents structural readability from being mistaken for semantic certainty. The version-by-version architecture is documented in <a href="technical-documentation/architecture.md">Technical documentation</a>.

<h2 id="engineering-approach" align="center">Engineering approach</h2>

Four distinctions guide the implementation:

- **Structure is not semantics.** A file can be parsed safely while some magnitudes remain uninterpreted.
- **Equipment is not necessarily the file writer.** Proprietary blocks may identify a software lineage without proving which OTDR acquired the trace.
- **Stored is not calculated.** Values or events added by analysis software remain distinguishable from information present in the measurement source.
- **Unknown is not corrupt.** Unsupported semantics should remain explicit instead of being silently guessed.

### Provenance-aware normalization

```python
normalized_field = {
    "value": ...,
    "raw_value": ...,
    "unit": ...,
    "source": ...,
    "rule_id": ...,
    "confidence": ...,
    "evidence": ...,
}
```

An empirical conversion, vendor-specific scale or inferred meaning therefore remains distinguishable from a value explicitly stored in the file.

<h2 id="current-capabilities" align="center">Current capabilities</h2>

The development snapshots documented through **v0.3.5** combine:

- safe inspection of SOR 2.00 structure and metadata;
- evidence-based selection of characterized vendor profiles;
- trace reconstruction from stored sample data;
- interactive local visualization of the OTDR curve;
- defensive `.EI` reading for the characterized CE6422 layout and evidence-based EI/SOR pairing;
- separation of SOR-stored events, EI-imported information, calculated candidates and manual review;
- contextual event analysis with explainable suppression and independent evidence;
- terminal diagnostics plus a calculated **possible non-reflective end** only when multiple terminal-evidence conditions agree;
- JSON export of the analysis model and CSV export of event data;
- graceful handling of partially supported variants without modifying the source measurement.

<h2 id="current-scope" align="center">Current scope · documented through v0.3.5</h2>

| Ecosystem | Status | Role in the project |
|---|---|---|
| **EXFO** | **Validated reference** | SOR 2.00 parsing, normalized parameters, stored events and trace visualization for the characterized reference profile. |
| **Ceyear CE6422** | **Active development** | Characterized trace interpretation, curve-calculated candidates, verified EI/SOR pairing for the observed layout, terminal diagnostics and evidence-backed end candidates. |
| **Yokogawa AQ1000** | **Structural** | File structure characterized; vendor-specific semantic normalization remains pending. |

<p align="center"><strong>structurally readable → profile identified → semantically characterized → empirically validated</strong></p>

<h2 id="event-provenance" align="center">Event provenance</h2>

Event origin is preserved as part of the model rather than flattened into one table:

- **SOR stored** — serialized event information such as `KeyEvents` when present;
- **EI imported** — records read from a supported EI layout, with verified-pair status kept explicit;
- **Calculated candidate** — hypotheses generated from the reconstructed curve, including supported terminal candidates;
- **Manual review** — user review/annotation state that does not alter the source measurement.

For the Ceyear family used during development, absence of `KeyEvents` means that the SOR contains **no stored event table**; it does not mean that the optical trace contains no events.

<h2 id="validation" align="center">Validation</h2>

Validation is layered rather than reduced to one pass/fail criterion:

1. structural and boundary checks;
2. synthetic or sanitized regression suitable for public migration;
3. private profile/reference regression;
4. trace reconstruction and determinism checks;
5. comparison with reference software or paired EI information where appropriate;
6. positive/negative controls for event and terminal logic;
7. manual validation of the local viewer and exports.

The archived v0.3.5 package records **76 tests passing**. Private operational traces and development corpora used for engineering comparison remain outside this public repository, and those comparisons are not presented as blind validation or metrological calibration.

<h2 id="data-handling" align="center">Data handling</h2>

OTDR Inside is designed as a **local, read-only workflow**. Source measurements are analyzed from local/temporary handling and are not overwritten by the viewer.

This repository does not distribute real operational `.sor`, `.ei` or `.otdr` measurements, customer or route identifiers, proprietary vendor executables, commercial manuals, licensed standards, or derived files that expose confidential trace metadata. Public examples and tests should rely on synthetic or explicitly sanitized data.

<h2 id="known-limitations" align="center">Known limitations</h2>

- `.EI` support is limited to the observed and characterized Ceyear layout; it is not universal EI compatibility. `.otdr` is not part of the normal processing path.
- Vendor-specific interpretation remains profile/signature based and should not be read as universal SOR compatibility.
- The displayed vertical trace level is relative and is not presented as universally calibrated optical power.
- A v0.3.5 non-reflective-end result remains a **calculated candidate**: it does not certify the physical fiber endpoint, event loss, reflectance, ORL or a metrological distance tolerance.
- OTDR Inside does not claim independent certification of Telcordia SR-4731 compliance.
- Unsupported semantics remain explicitly unresolved rather than being assigned speculative values.

<h2 id="roadmap" align="center">Roadmap</h2>

- validate event and terminal classification against additional independent references;
- extend paired-format and vendor coverage without weakening provenance checks;
- compare paired wavelengths and multi-trace behavior;
- add batch processing, duplicate detection and anomaly review;
- expand semantic support for additional vendor profiles;
- build a compatibility matrix based on reproducible validation evidence.

<h2 id="author" align="center">Author</h2>

<p align="center">
  <strong>Esteban Erazo</strong><br>
  Mechatronics Engineering · Universidad Nacional de Colombia<br><br>
  <a href="https://github.com/EstebanErazo500"><img src="assets/nav/profile.svg" alt="@EstebanErazo500"></a>
</p>
