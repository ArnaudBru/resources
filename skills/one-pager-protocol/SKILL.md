---
name: one-pager-protocol
description: Create a practical HTML one-pager reference document following established conventions. Walks through pre-flight questions, applies structural and visual conventions, produces a self-contained HTML file at ~/Desktop/plans/html/<slug>.html. Use when the user says "make me a one-pager", "create a one-pager", "write me a cheat-sheet", "document this as a reference page", "make a quick reference for X", or similar requests for a scannable HTML reference document.
---

# Practical one-pager protocol

Build a 1–2 page HTML reference doc worth coming back to. Output is a single self-contained `.html` file.

The protocol is **inductive**: do not aim for perfection upfront. Ask the user's intent, build a v0, iterate.

Canonical reference (the protocol applied to itself): `practical_one_pager_protocol.html` (sibling file in this skill's directory) — read it for the visual conventions and the worked example. The instructions below are the procedural shell; that HTML is the substance.

## Process

### Phase 1 — Pre-flight (always, before opening an editor)

Use `AskUserQuestion` to surface 5 decisions. Wrong answers force a full rewrite — cheap to ask, expensive to skip. Infer answers the user has already implied in their request; only ask what's missing.

1. **Audience** — me only / team / mixed (self + team + future hires) / external
2. **Usage context** — on-screen during debugging / printed / linked from tickets / onboarding
3. **The question this page answers, in one sentence** — this becomes the title
4. **Specificity** — generic body / generic body + example sidebar / project-coupled
5. **Shape** — decision-guide / reference / analysis-and-recommendation

### Phase 2 — File setup

Save at `~/Desktop/plans/html/<slug>.html`. Slug derives from the question the page answers, not the underlying concept.

**Tech baseline** (non-negotiable):

- Single self-contained `.html` file
- Inline `<style>` only; **no external CDN**; **no JS dependency**; **no build step**
- Must work offline (else fails the on-screen-during-debugging use case)
- Markdown-readable structure where reasonable (headings, paragraphs, lists); hand-crafted HTML only for tables and visual callouts that earn their height

### Phase 3 — Structure (top to bottom)

| Block | Status | Notes |
|---|---|---|
| Title + 1-line subtitle | required | The title IS the question. Subtitle says what the page answers in one sentence. |
| TL;DR box | required for analyses / optional for pure references | 3–5 bullets. Includes any dominant formula's full variable names. Readers screenshot this. |
| Framework / decision tree | required for decision-guides | Numbered lenses or steps applied in order. |
| Reference tables | required | Core scan layer. Comparison tables, formula tables, etc. |
| Worked example | required | One named example. Boxed sidebar with "EXAMPLE" label. Clearly skippable. |
| Pitfalls / gotchas | required | Specific failure modes + how to recognise + how to fix. |
| References / further reading | optional | Three pointers, five max. Else delete the section. |

### Phase 4 — Density and content rules

- **Each idea in exactly one place.** Don't write a separate "definitions" section that re-states what the formula table already says — add a "What it is" cell to the table.
- **Tables for parallel content** (4+ rows of same shape); **prose for causality** ("this happened because…"); **bullets for unstructured enumerations**.
- **Variable names match across TL;DR, tables, and examples.** Never use single-letter abbreviations without a legend. If the TL;DR says `num_workers`, the formula table must say `num_workers` — not `w`.
- **Pair formula tables with concrete-priors tables.** Formulas give dimensions; numbers give magnitudes. Both are needed.
- **Replace metaphors with measurements.** If a number can carry the claim, let it.
- **Hedge consistently** — if one scaling claim is hedged, hedge all. Don't mix "linear" and "proportional" for the same math.
- **Call out non-obvious context-dependent shifts** — places where the same formula has different dominant terms depending on the case.
- **Subcategorise multi-option lists by mechanism.** When options differ in *what they fix* (root cause vs symptom vs cost), group by mechanism — don't flatten into a single ranked list.
- **Generic body, project example boxed.** The main flow reads generically; one labelled "Example" sidebar anchors it in a real case.
- **Match paragraph styling to paragraph purpose.** Muted / smaller text is for *framing* intros (e.g., "what you spend vs what you get" above a comparison table). Body / ink / full size is for *substantive content* (e.g., a paragraph that names 5 categories the reader needs to recognise). Don't bury content under framing styles — readers will skim past it.

### Phase 5 — Visual conventions

Reuse the palette and callout styles from `practical_one_pager_protocol.html` (sibling file in this skill's directory) — copy the CSS skeleton from there. **Don't invent new visual styles per page.**

Standard callout blocks (one style per block type):

| Block | Visual |
|---|---|
| TL;DR | Card with left border in `--ink` |
| Example | Dashed border, light-grey background, "EXAMPLE" pill label |
| Pitfalls / gotchas | Amber-tinted box, left border in `--warn` |
| What to cut | Red-tinted box, left border in `--bad` |

Layout:

- **Two-column layout ONLY for genuinely parallel concepts.** Default to single-column flow.
- **Sequential frameworks** → numbered section circles, single-column.
- **Section accent colours** only when they map onto a real distinction in the content — never for visual variety.
- **CSS Grid for column alignment; Flex for row-level rhythm.** Use Grid only when multiple rows genuinely need to align in columns (column structure is load-bearing). Use Flex when each row is independent and you just want consistent inline spacing between items. Grid's `1fr` columns create invisible whitespace between text-end and column-end that the eye reads as "inconsistent spacing between words" — a common bug when content widths vary across rows.

### Phase 6 — Iterate

Open the page: `firefox <path> 2>/dev/null &` (Firefox per the user's CLAUDE.md, not Chrome / `xdg-open`).

Then invite feedback on specific dimensions, e.g.:

- Does the title match the question they actually want answered?
- Are all decision-variable parameters visible in the TL;DR?
- Any single-letter variable mysteries in the formula tables?
- Each idea in exactly one place, or is there duplication to cut?
- Does the example anchor the framework without overshadowing it?

Apply feedback. **Each iteration may surface a new rule worth extracting.** If a rule generalises beyond the specific page, propose adding it to the canonical protocol on the next stable build.

## Common pitfalls

Things that look right when writing them but rot the page:

- **A separate "definitions" section** that duplicates the formula table.
- **Concept-name titles** ("X Estimation", "Y Guide"). Find the question the reader is asking; title accordingly.
- **Single-letter variables without a legend.** Obvious to the writer, opaque to the reader.
- **Metaphors when measurements exist.**
- **"Useful tips" piles** that aren't a real grouping. If you have 8 tips, check whether they cluster into 2–3 categories first.
- **Project narration in the main flow.** Project-specific anecdotes belong in the labelled Example sidebar, not in paragraph 3 of the framework.
- **External JS or CDN dependencies.** If the page needs the internet to render, it fails the offline use case.
- **A "Further reading" list of every link you've ever seen.** Three to five pointers, or delete the section.

## Example output

The canonical worked example is `practical_one_pager_protocol.html` (sibling file in this skill's directory) — the protocol applied to itself. Read it for the CSS skeleton, the exact callout block markup, the colour palette, and a worked example of the pre-flight question → final page mapping.

Other pages produced under this protocol live in `~/Desktop/plans/html/` and can serve as additional reference points.
