<h1 align="center">Event provenance · documented model</h1>

<p align="center">
  <a href="../README.md"><img src="../assets/nav/project-home.svg" alt="Project home"></a>
  <a href="README.md"><img src="../assets/nav/technical-docs.svg" alt="Technical docs"></a>
  <a href="architecture.md"><img src="../assets/nav/architecture.svg" alt="Architecture"></a>
  <img src="../assets/nav/event-provenance-current.svg" alt="Event provenance · current page">
</p>

<p align="center">
  <a href="event-provenance.md"><img src="../assets/nav/lang-en-selected.svg" alt="English"></a>
  <a href="event-provenance.es.md"><img src="../assets/nav/lang-es.svg" alt="Español"></a>
</p>

<p align="center"><strong>Origin is part of the data. Confidence and review are separate dimensions.</strong></p>

> **Scope.** This page documents the provenance model represented by the archived releases currently reconstructed through **v0.3.5**. It describes how OTDR Inside keeps stored, imported, calculated and manually reviewed information distinguishable. Later releases may extend the model, but should preserve the historical meaning of these categories.

OTDR analysis becomes ambiguous when values that merely appear together in a viewer are treated as if they came from the same source. OTDR Inside avoids that collapse by carrying **where a value or event came from** alongside the value itself.

<p align="center">
  <img src="../assets/provenance/model-light.svg#gh-light-mode-only" alt="OTDR Inside event provenance model in light mode" width="100%">
  <img src="../assets/provenance/model-dark.svg#gh-dark-mode-only" alt="OTDR Inside event provenance model in dark mode" width="100%">
</p>

## Four conceptual origins

| Origin | What it means | What it does not mean |
|---|---|---|
| **SOR stored** | The value or event is serialized in the original SOR structure, for example a `KeyEvents` table when present. | It is not automatically a universal ground truth for every vendor or semantic field. |
| **EI imported** | The record was read from a supported EI layout. When shown as a verified companion, the EI/SOR pairing checks succeeded. | Imported EI data does not overwrite SOR data and is not silently promoted to stored SOR information. |
| **Calculated candidate** | The analysis pipeline derived a hypothesis from the reconstructed trace and its surrounding evidence. | It was not serialized by the instrument merely because it is displayed beside stored events. |
| **Manual review** | A human reviewer added assessment state, comments or annotations during the analysis session. | Human review does not rewrite the original measurement or change the historical origin of the underlying event. |

The model deliberately does **not** rank these origins from “best” to “worst.” They answer different questions. Provenance records **source**, while evidence, semantic support and review describe other dimensions.

## Provenance, evidence, review and confidence are not synonyms

OTDR Inside keeps four related concepts separate:

| Dimension | Question it answers |
|---|---|
| **Provenance** | Where did this information come from? |
| **Evidence** | What measurements, relationships or local context support the interpretation? |
| **Review state** | Has a person reviewed the item, and what was the decision? |
| **Semantic confidence/support** | How strongly is the displayed meaning supported for this format/profile? |

A calculated candidate may have strong local evidence and still remain **calculated**. A stored event may be serialized in the source and still require vendor-specific semantic interpretation. A manually accepted candidate remains a calculated candidate with an accepted review state; review does not rewrite provenance.

## SOR path · stored information and reconstructed trace

The SOR path can expose two different kinds of information at the same time:

1. **Stored event information**, when the file actually contains an event structure such as `KeyEvents`.
2. **Trace samples**, which can be reconstructed and analyzed independently of whether an event table exists.

This distinction is especially important for the characterized Ceyear family. The absence of `KeyEvents` means **no stored KeyEvents table was found in that SOR**. It does not demonstrate that the optical trace contains no events.

Calculated analysis therefore remains downstream of trace reconstruction and receives its own provenance instead of being inserted into the stored-event category.

## EI path · imported information with explicit pairing state

From v0.3.3, `.EI` becomes a second input path. EI information keeps its own origin whether the file is opened independently or used as a verified companion.

For the characterized EI/SOR family, pairing is not accepted by filename resemblance alone. The implementation checks the observed sample relationship together with acquisition metadata before treating the EI as a verified companion source.

That distinction matters in the viewer:

- **EI opened alone** — records may be inspected as EI-imported information, while the session states that no SOR pair has been verified.
- **Verified EI/SOR pair** — EI-imported information can be compared with the SOR and calculated analysis, but its origin remains EI.

Pair verification strengthens the relationship between the files; it does not merge their provenance.

## Calculated candidates · analysis stays visibly calculated

The 0.3 line introduces event hypotheses derived from the reconstructed curve. Detection, contextual refinement and evidence analysis may improve or suppress a proposal, but none of those stages converts it into information that was stored by the instrument.

The intended lifecycle is conceptually:

`trace → proposal → contextual analysis → evidence → calculated candidate → optional human review`

Suppressed candidates may retain diagnostic reasons and feature context so that a negative decision remains explainable rather than disappearing silently.

## v0.3.5 · terminal evidence without provenance inflation

v0.3.5 adds a separate terminal-evidence stage around the terminal diagnostic region. A transition can be promoted to a **possible non-reflective end candidate** only when the terminal evidence used by the implementation aligns, including the relevant edge/transition behavior, relative drop and persistent post-event noise context.

If that evidence does not align, the region remains diagnostic **D** rather than being forced into an event label.

Most importantly, a promoted terminal result still has **calculated provenance**. The additional evidence changes the support for the hypothesis; it does not turn the hypothesis into a stored instrument event or a certified physical fiber end.

The documented model therefore preserves this distinction:

`terminal diagnostic D → evidence gate → possible non-reflective end candidate (CALCULATED)`

not:

`terminal diagnostic D → certified fiber end`

## Human review · assessment overlays the source, it does not replace it

Human review was separated explicitly from detection in v0.3.1. Review state can record values such as `pending`, `accepted` or `rejected`, together with comments and history.

This produces combinations such as:

| Provenance | Review state | Interpretation |
|---|---|---|
| Calculated candidate | Pending | Algorithmic proposal awaiting human assessment. |
| Calculated candidate | Accepted | Algorithmic proposal accepted by a reviewer; still calculated. |
| Calculated candidate | Rejected | Proposal retained historically but not accepted by the reviewer. |
| EI imported | Reviewed | Imported EI record with an additional human assessment; origin remains EI. |
| SOR stored | Reviewed | Stored source information with reviewer context; origin remains SOR. |

The review layer is additive. It never mutates the source measurement.

## How the provenance model evolved

| Release | Provenance-related change |
|---|---|
| **v0.1.0** | Stored EXFO events and structural/normalized information are already distinguished from the raw file representation. |
| **v0.3.0** | Curve-derived candidates become an explicit calculated source rather than being mixed with stored events. |
| **v0.3.1** | Evidence and human review become separate layers around calculated candidates. |
| **v0.3.3** | EI becomes a second imported source with explicit pairing state. |
| **v0.3.5** | Terminal evidence can support a possible non-reflective end candidate while keeping the result in the calculated origin. |

## Presentation and export contract

The viewer and public migration should preserve several invariants:

- the origin of an event remains visible when different sources are displayed together;
- stored and calculated records are not silently merged into one semantic category;
- EI pairing state remains distinguishable from EI provenance itself;
- human review adds state and commentary without rewriting source origin;
- raw values and unsupported semantics remain available where the model supports them;
- terminal evidence can strengthen a calculated hypothesis without converting it into a certified physical claim.

These rules apply to UI representation and to analysis/event exports so that provenance survives beyond the screen where the event was first inspected.

## What this model does not claim

Provenance is not a truth score. A stored value is not automatically more physically correct than every calculated one; an accepted review is not a new measurement; an EI import is not equivalent to a SOR-stored event; and a v0.3.5 terminal candidate is not a calibrated declaration of the physical fiber end.

The purpose of provenance is narrower and more useful: **make the origin of each piece of information explicit enough that later interpretation, validation and review do not erase how it was obtained.**

---

<p align="center">
  <a href="README.md"><img src="../assets/nav/technical-docs.svg" alt="Technical docs"></a>
  <a href="architecture.md"><img src="../assets/nav/architecture.svg" alt="Architecture"></a>
</p>

<p align="center"><sub>Next standalone documentation page: Validation.</sub></p>
