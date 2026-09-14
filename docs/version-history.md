# Version history

> This public history is reconstructed from archived development snapshots. It documents the real sequence of technical changes, but it does **not** pretend that those snapshots were originally created as Git commits. Public tags will be added as the sanitized code migration progresses.

[Español](version-history.es.md)

## Evolution at a glance

| Version | Engineering focus | Main change |
|---|---|---|
| **v0.1.0** | First vertical slice | Safe SOR parsing, profile-based interpretation, trace reconstruction, local viewer, stored EXFO events, JSON/CSV export and automated tests. |
| **v0.1.1** | Windows startup robustness | Startup diagnostics, Python-version checks and automatic fallback when the default local port is unavailable. |
| **v0.1.2** | Windows security compatibility | Shell launchers removed in favor of a VS Code task compatible with Smart App Control; the SOR engine and profiles remained unchanged. |
| **v0.2.0** | Ceyear curve characterization | Ceyear CE6422 curve axes validated against reference software; capability-level evidence and explicit semantic limits added. |
| **v0.3.0** | Calculated event candidates | Experimental curve-based Ceyear event detection added while preserving stored/calculated provenance and raw serialized samples. |
| **v0.3.1** | Evidence and human review | Exportable evidence windows, local noise/context diagnostics and an explicit pending/accepted/rejected review workflow. |
| **v0.3.2** | Contextual hybrid detector | Persistence, polarity, recovery context and terminal-region logic added; suppressed candidates remain explainable in exported diagnostics. |
| **v0.3.3** | EI/SOR paired analysis | Defensive `.EI` reading, strict EI/SOR pairing by complementary samples and acquisition metadata, and separate event provenance for EI information. |
| **v0.3.4** | Terminal diagnostics and localization | Navigable D1 terminal region, experimental multiscale localization and regression focused on terminal behavior. |

## Phase 1 — Stable structural baseline

### v0.1.0

The first archived release already formed a complete vertical slice rather than a parser-only experiment. It included:

- read-only local `.SOR` loading;
- structural scanning through the SOR map and named blocks;
- explicit profile selection based on available evidence;
- an EXFO FTB-7200D profile used as the validated reference baseline;
- full trace reconstruction and interactive visualization;
- stored EXFO events overlaid on the trace;
- acquisition parameters, identification, structure and provenance views;
- JSON analysis export and CSV event export;
- a preliminary Ceyear CE6422 profile;
- controlled degradation for files without a validated semantic profile.

The archived v0.1.0 changelog records **23 automated tests passing**.

### v0.1.1

The analysis engine remained essentially the same. Development shifted to operational robustness on Windows:

- startup diagnostics were added;
- Python 3.11+ was checked explicitly;
- failures remained visible instead of closing the terminal immediately;
- the viewer could try additional local ports when the default port was occupied.

### v0.1.2

The next change was deployment-related rather than analytical. Shell launchers were removed and startup moved to an integrated VS Code task so the workflow could coexist with Windows Smart App Control without asking the user to disable security controls.

The archived changelog explicitly states that the **SOR engine, profiles and read-only rules remained unchanged**.

## Phase 2 — Vendor semantics become explicit

### v0.2.0

Ceyear support moved from preliminary structural support to an empirically characterized curve profile.

The archived release documents:

- horizontal and vertical curve validation against OTDR_PC using controlled reference files;
- optional regressions over a larger private Ceyear corpus without packaging those traces with the code;
- capability-level status instead of a single binary “supported / unsupported” label;
- a distinction between nominal range and sample extent;
- explicit acknowledgement that event information visible in reference software was not necessarily serialized as `KeyEvents` in the observed SOR files;
- relative recovered trace level instead of presenting the vertical axis as universally calibrated optical power;
- evidence attached to normalized capabilities in the exported JSON model.

This release is the point where **structural readability and semantic certainty become deliberately separate concepts in the product model**.

## Phase 3 — Event analysis with provenance

### v0.3.0

The 0.3 line begins with calculated event analysis for the characterized Ceyear trace family.

Key changes include:

- an experimental detector for peaks and persistent level transitions;
- an explicit distinction between the serialized sample region and the useful analysis region;
- calculated candidates kept separate from events stored in the original SOR;
- candidate selection synchronized with the viewer;
- manual annotations with history;
- relative diagnostic quantities instead of invented physical magnitudes;
- local-only serving on `127.0.0.1`.

### v0.3.1

Rather than replacing the baseline detector simply because alternative thresholds produced different outputs, v0.3.1 preserved the 0.3.0 generator when the tested alternatives did not improve the available references.

The release instead added an **evidence layer**:

- multiple window sizes and positions;
- local-noise measurements;
- neighboring context and reasons;
- highlighted before/after windows in the viewer;
- a separate human-review state (`pending`, `accepted`, `rejected`) with comments and history;
- exported evidence and review state.

This separation is important: automated detection and human validation are represented as different pieces of information.

### v0.3.2

The hybrid detector expanded the baseline with contextual logic:

- event polarity;
- local persistence;
- recovery context;
- a separate terminal/noise region;
- a limited number of additional persistent-change proposals;
- rejected or suppressed candidates retained with the rule and feature vector that caused the decision.

The development comparison recorded in the archived release reports improved recovery of the available intermediate references, while explicitly noting that those references were **not blind evaluation data**. That limitation is preserved in the public history rather than converted into an accuracy claim.

## Phase 4 — Paired formats and terminal diagnostics

### v0.3.3

This release introduced defensive `.EI` parsing and paired EI/SOR analysis.

A pair is not accepted by filename alone. The archived implementation requires agreement between acquisition metadata and the expected complementary sample relationship for the characterized Ceyear export family.

The model also separates event origin into distinct categories, including:

- information stored in the SOR;
- information imported from a verified EI pair;
- curve-calculated candidates;
- manual review or annotation.

The Ceyear export signature was broadened from one exact file to the verified structural family, while raw values and unsupported semantics remained preserved.

### v0.3.4

The latest archived snapshot focuses on terminal behavior and localization diagnostics:

- a visible and navigable **D1 terminal region** distinct from physical event candidates and EI comparison;
- CSV/JSON exports aligned with the diagnostic and localization model;
- a multiscale ramp-localization experiment that was evaluated without replacing the primary positions when it did not outperform the controlled comparison;
- dedicated regression around terminal behavior, determinism and display reduction.

The localization code explicitly treats multiscale spread as **algorithm sensitivity, not a statistical confidence interval**.

## What remained stable

Two important baseline components are byte-identical across every archived snapshot from **v0.1.0 through v0.3.4**:

- `scanner.py` — the structural SOR scanner;
- `exfo_ftb7200_sor2.json` — the validated EXFO reference profile.

That continuity is useful context for the public migration: later versions primarily add semantic interpretation, evidence, event analysis, EI support and viewer behavior on top of a stable structural core.

## Public migration policy

The Git history in this repository will be reconstructed from these archived snapshots with the following constraints:

1. operational `.SOR`, `.EI` and `.otdr` measurements are never committed;
2. private routes, serial numbers, corpus filenames, hashes and internal paths are removed or generalized;
3. proprietary vendor software, licensed standards and commercial manuals are not redistributed;
4. public regression uses synthetic or explicitly sanitized fixtures;
5. tags represent sanitized historical states, not fabricated original commit dates.

The intended public sequence is:

`v0.1.0 → v0.1.1 → v0.1.2 → v0.2.0 → v0.3.0 → v0.3.1 → v0.3.2 → v0.3.3 → v0.3.4`

---

[Back to documentation index](README.md) · [Back to project README](../README.md)
