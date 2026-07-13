# Comparative Scorecard

The point of this section is **comparability**: every audit ends with the same
fixed-pattern scorecard, computed the same way, so that separate GrowGoSnipe runs
can be lined up against each other and a company can be placed against its sector.
It is emitted for Company Snipe by default, and for any multi-source run on
request. The headline question it answers: *of everything we assessed, how much
was misleading or contradicted, and how does that compare to peers?*

Comparability only holds if the metric is computed identically every time. Do not
improvise the definitions below.

## The strict metric

The scorecard is computed at **two levels**, so a reader can see both *how much
material was examined* and *how much of what was examined actually carried
checkable facts*. Report both — a low extraction yield is itself a finding (a
prolific target that publishes a lot but says little that is checkable).

### Source-level funnel (how much was looked at)

Work from the source manifest (every collected item — see
`source-manifest-schema.md`).

- **Sources considered** = every distinct item collected in the window across all
  tracks (each post, page, report, filing, statement), **before** deduplication.
  This is the total volume examined.
- **Sources deduped** = items remaining after collapsing `duplicate_group`s
  (syndicated reposts, cross-posted press releases, language duplicates). The base
  of distinct material.
- **Sources with claims (extracted)** = deduped items with
  `detected_claim_count ≥ 1` — items that actually yielded at least one checkable
  factual claim. Everything else (pure advocacy/opinion, PR, event notices,
  greetings, images with no factual assertion) is examined but carries nothing to
  verify.
- **Extraction yield** = `Sources with claims / Sources considered` — what share
  of collected material was factually checkable. Report it with the raw counts,
  e.g. "Extraction yield 3% (11 of 338 items carried checkable claims)".

Never silently drop the gap between *considered* and *with claims*: that gap is
the honest answer to "you looked at N things but only checked a few" — surface it,
don't hide it.

### Claim-level metric (how much held up)

Work from the full claim ledger (the raw CSV — every extracted claim).

- **Extracted** = every claim on the ledger.
- **Assessed** = claims that received a substantive, evidence-based verdict:
  Supported, Mostly supported, Mixed, Misleading, Unsupported, Contradicted,
  Outdated. **This is the denominator.**
- **Excluded from the denominator:** `Deferred` (not checked this pass) and
  `Unverifiable` (cannot be checked in principle). Including them would make the
  rate depend on how much you happened to verify, which destroys comparability.
  Report their counts separately so the exclusion is transparent.
- **Adverse** = `Misleading` + `Contradicted`. These are the two verdicts the
  scorecard is built around.

Headline metric — the **Adverse rate**:

```
Adverse rate = (Misleading + Contradicted) / Assessed
```

Report it as a percentage with the raw counts and the denominator visible, e.g.
"Adverse rate 18% (7 of 39 assessed; 4 Misleading, 3 Contradicted). Excluded from
denominator: 6 Deferred, 2 Unverifiable." Never show the percentage without the
counts and denominator — a bare "18%" is not comparable or checkable.

Also report, as secondary lines (not the headline):
- **Source funnel** = `Sources considered → deduped → with claims`, plus the
  **extraction yield**. This shows how much raw material sits behind the claim
  ledger; a large considered-vs-with-claims gap is what explains a target with
  hundreds of posts producing only a handful of checkable claims. Always show the
  three raw counts, not just the yield percentage.
- **Unsupported rate** = `Unsupported / Assessed` — high unsupported is a
  different problem (thin sourcing) from high adverse (active inaccuracy/spin).
- **Coverage** = `Assessed / Extracted` — how much of the ledger was actually
  judged. A low coverage means the adverse rate rests on a small base; say so.

## Fixed reporting bands

Map the adverse rate to a fixed band so audits sort consistently. These bands are
a **reporting convention for consistency, not an empirical claim about any
sector** — the sector comparison below is separate and must be evidence-based.

| Adverse rate | Band |
|---|---|
| 0–5% | Clean |
| 5–15% | Low concern |
| 15–30% | Elevated |
| 30–50% | High |
| >50% | Severe |

State the band as "on GrowGoSnipe's fixed convention" so no one mistakes it for a
measured sector norm.

## Sector positioning — evidence-based only, never invented

This is where the temptation to hallucinate is highest. Hard rules:

- **Never fabricate a sector average, median, or percentile.** "The fintech
  sector averages 12%" stated without a real source is exactly the kind of
  confident-but-ungrounded number the flag-integrity check exists to catch.
- A sector comparison may be made **only** from a real basis, and you must name
  which one:
  1. **Peer audits the user has actually run** — prior GrowGoSnipe scorecards on
     companies in the same sector, supplied by the user or accumulated from the
     standardized summary records (below). Compare like-for-like and say how many
     peers the comparison rests on ("above the median of the 4 peer audits on
     file").
  2. **Cited external data** — a real, retrieved study/benchmark, cited. Subject
     to the normal source hierarchy and copyright limits.
- **If neither basis exists, say so plainly and stop there:** report the company's
  own metric and band, and state "no sector baseline available — this is a
  baseline datapoint." Do not manufacture a comparison to fill the gap. A single
  honest datapoint that seeds future comparison is the correct output, not a
  fake ranking.
- Record the company's **sector** explicitly (how it was classified and on what
  basis), so future comparisons are like-for-like and not apples-to-oranges.

## Standardized summary record — how the sector dataset gets built

So that runs actually become comparable over time, every scorecard also emits one
fixed, machine-readable summary line, written to
`/mnt/user-data/outputs/grow-go-snipe-scorecard-[target]-[YYYY-MM-DD].csv` and
surfaced in the report. Fixed fields, in order:

```
audit_date, target, sector, sources_considered, sources_deduped,
sources_with_claims, extraction_yield, extracted, assessed, misleading,
contradicted, adverse_rate, unsupported_rate, coverage, band, composite_score,
skill_version
```

The first four data fields after `sector` are the **source funnel**
(`sources_considered`, `sources_deduped`, `sources_with_claims`,
`extraction_yield`); the rest are the claim-level metric. `extraction_yield` is
`sources_with_claims / sources_considered` as a fraction. When a run genuinely has
no source manifest (a single pasted text with no collection stage), set the funnel
fields to the item count itself (considered = deduped = with_claims = 1 if it
carried a claim) rather than leaving them blank, so the column count stays fixed.

**Schema note:** this row now has 17 fields (the four source-funnel fields were
added after `sector`). Bump `skill_version` when you change the schema so 17-field
rows are never silently lined up against older 13-field rows — a differing
`skill_version` is the signal not to compare column-by-column.

Tell the user this line is the unit of cross-audit comparison: collecting these
rows across companies is what makes the sector benchmark real. The skill does not
keep a persistent store between conversations, so the user (or their pipeline)
owns that running dataset — the skill's job is to emit a clean, consistent row
every time and to compare only against rows actually provided back to it.

## Guardrails

- The scorecard is **downstream of the verdicts** — it counts them, it never
  changes them. Don't re-open a verdict to move the rate.
- Include `skill_version` in the summary so rates from different skill versions
  aren't silently compared as if identical.
- If coverage is low or the assessed base is tiny (e.g. <10 assessed claims), the
  rate is noisy — report it, but flag that it's a small-sample number and
  shouldn't carry a confident sector verdict.
