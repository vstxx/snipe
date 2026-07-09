# Claim Taxonomy

The purpose of this phase is to convert prose into a clean inventory of
**checkable factual claims**, stripped of the editorial packaging they arrive
in. Everything downstream depends on this being done carefully: a claim you fail
to extract is a claim you never audit.

## The checkability test

A statement is a checkable factual claim if it asserts something about the world
that could, in principle, be confirmed or refuted by evidence. Ask:

1. **Does it assert a state of affairs?** (Not a preference, a value, or a call
   to action.)
2. **Could evidence settle it?** Even if you can't find the evidence right now,
   is it the *kind* of thing evidence bears on?
3. **Is it specific enough to check?** "Inflation is high" is nearly
   uncheckable; "inflation reached 6.2% in March 2024" is fully checkable. Vague
   claims get extracted but flagged as under-specified.

If yes to 1 and 2, extract it. If it fails the test, classify it as non-factual
(below) and set it aside — but note rhetoric that is *doing the work of* a
factual claim without committing to one.

## What to extract vs. set aside

| Category | Extract? | Example |
|---|---|---|
| Factual claim | Yes | "The plant employs 1,200 people." |
| Statistical claim | Yes | "Exports rose 12% year-on-year." |
| Causal claim | Yes | "The subsidy cut the deficit." |
| Attribution/quote | Yes | "The minister said the law would pass." |
| Prediction | Yes (flag as forecast) | "GDP will contract next year." |
| Opinion / value judgment | No | "This is a disastrous policy." |
| Normative claim | No | "The government should resign." |
| Rhetorical question | No (but note) | "How much longer can this go on?" |
| Pure hypothetical | No (but note) | "If rates rose, borrowers would suffer." |

The distinction matters because a common failure mode of bad reporting is
smuggling a factual claim inside an opinion. Extract the factual core and leave
the editorial wrapper behind:

- Text: "The government's reckless €4bn giveaway to its cronies…"
- Extract: (a) a program exists worth €4bn [checkable]; (b) recipients are
  connected to the government [checkable, and likely the load-bearing claim];
  (c) "reckless" / "cronies" [rhetoric, not extracted, but note that (b) is
  being asserted rhetorically rather than evidenced].

## Claim types

Classify each extracted claim. The type drives how you verify it and what
counts as adequate evidence.

**Statistical / quantitative** — numbers, percentages, rates, rankings,
magnitudes, trends. Highest risk of error and manipulation. Always trace to the
data producer. Watch for: wrong base, percentage vs. percentage-point
confusion, nominal vs. real, cherry-picked start dates, absolute vs. per-capita.

**Causal** — asserts X produced/caused/led to Y. The hardest type to verify;
correlation is routinely dressed as causation. Ask whether the source
established a mechanism or just a co-occurrence. Rarely deserves a clean "True"
unless a rigorous study supports it.

**Historical / event** — X happened, on date Y, in place Z. Usually verifiable
against records. Watch for wrong dates, conflated events, and misremembered
sequence.

**Attribution / quote** — person or body P said/wrote/reported Q. Two separate
checks: (1) did P actually say it, and (2) is what P said itself true? A quote
can be accurate but the underlying claim false, or vice versa. Locate the
original utterance, not a paraphrase of a paraphrase.

**Predictive / forecast** — X will happen. Cannot be verified as true or false
in the present, so **split every forecast into two claims**:
- an **attribution claim** — did the named forecaster actually issue this
  forecast, with these figures? This half *is* verifiable now (trace it to the
  forecaster's own publication) and gets a normal verdict.
- a **forecast quality assessment** — is the forecaster credible, are the stated
  assumptions reasonable, does the forecast fall within the range of other
  serious projections, and is any uncertainty disclosed? This half is rated on
  provenance and plausibility, never on truth.

Keep the two visibly separate in the report. "The forecast is accurately
attributed to the central bank, but rests on an oil-price assumption well
outside consensus" is a complete and useful finding that a single true/false
verdict would flatten.

**Comparative** — X is bigger/faster/more than Y. Check that both sides are
measured the same way and over the same period; mismatched comparisons are a
common sleight of hand.

**Definitional / categorical** — X is a type of Y; X qualifies as Z. Often
turns on a definition or legal threshold. Verify against the governing
definition (statute, standard, official classification).

**Existence** — a study/report/law/event exists. The cheapest to fake and a
frequent tell. "A Harvard study found…" with no citation is an existence claim
that must be independently confirmed before its contents are even discussed.

## The claim_maker field (required for every claim)

For each extracted claim, record **who is making it**. This is not the same as
who the claim is *about*, and it is central to attribution analysis: a figure
asserted in the article's own voice carries different weight and risk than the
same figure attributed to a named statistics office or to "unnamed experts".

Use exactly one of these values:

- **article/editorial voice** — asserted by the text itself, unattributed.
- **quoted person** — attributed to a named individual (record who).
- **referenced report** — attributed to a specific named study/report/dataset.
- **named institution** — attributed to a named body (agency, company, NGO).
- **unnamed experts** — laundered attribution ("experts say", "studies show",
  "sources report") with no identifiable source. This value is itself an
  integrity signal.
- **unclear** — attribution is ambiguous or can't be determined from the text.

The claim_maker drives later steps: an "unnamed experts" or "article/editorial
voice" origin for a load-bearing claim raises the bar for corroboration and
often triggers an attribution flag; a "named institution" origin tells you which
primary source to trace to.

## Output of this phase

A numbered claim inventory. For each claim record: an ID, the claim restated
neutrally in your own words, its type, its **claim_maker**, and its **origin** —
a hard link to the exact source the claim was extracted from (`claim_source_url`)
plus where inside that source it appears (`claim_source_locator`: a text-fragment
anchor `#:~:text=…` for web pages, a paragraph/section reference for documents, a
timestamp for video, or a post permalink for social). Origin is where the claim
was *made*; it is not the evidence it will later be checked against — keep the two
separate. When the input is pasted text with no URL, leave the link unknown and
give a locator only (paragraph/section); never invent a link. Do not verify yet —
extraction and verification are separate passes, and mixing them causes you to
under-extract.
