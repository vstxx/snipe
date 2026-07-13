# AI-Style Signal (advisory)

This is a **narrow, hedged, opt-in-by-trigger** pass. It is not the point of
GrowGoSnipe and it never becomes the deliverable. The skill audits whether claims
are true and well-evidenced; **the truth of a claim does not depend on whether a
human or a machine wrote the text around it.** This pass exists only to give the
reader a weak, clearly-labelled stylistic heads-up on flagged material — nothing
more.

Read this whole file before running the pass. If you can't run it within these
constraints, don't run it.

## Non-negotiable constraints

1. **Never a binary verdict.** Do not output "this is AI-written" or "this is
   human-written". The output is a *signal with a confidence band*, always
   hedged, always reversible.
2. **Never accuse a named real person.** Attributing AI authorship to an
   identifiable individual is a factual claim about that person that can be false
   and damaging. Describe the *text's* style; do not assert what a named author
   did. No "X used AI to write this."
3. **Isolated from scoring.** The signal never enters the four axis scores or the
   Composite GrowGoSnipe Score. Keep it in its own section, after the factual
   audit, and state explicitly that it does not affect the scores.
4. **Isolated from verdicts.** A claim's verdict (Supported / Contradicted / …)
   is decided on evidence only. The AI-style signal must not nudge any verdict up
   or down.
5. **Trigger only.** Run this pass **only on flagged items** — items carrying a
   claim already marked high-risk / flagged by the normal audit. Do not run it on
   every item, and do not go looking for reasons to run it. If nothing was
   flagged, this section does not appear.
6. **Same provenance discipline as claims.** If you report a marker, say where it
   occurs (locator), just like a claim's origin.

## Why the heavy hedging

AI-style detection by surface markers is genuinely unreliable: it produces
frequent false positives, it penalizes careful non-native and edited human
writing, and the markers drift as models and editing tools change. Treat every
signal as *a stylistic observation that is consistent with, but not proof of,
machine generation.* When the honest answer is "inconclusive", say inconclusive —
that is the most common correct output here.

## What to actually look at

Observable, describable stylistic markers — report the ones present, with a
locator, not a checklist score:

- Uniform, template-like paragraph structure and rhythm; low sentence-length
  variance.
- High density of hedged, non-committal connective phrasing and list-like
  scaffolding where a human would vary.
- Formulaic contrastive constructions used repeatedly ("it's not just X, it's Y";
  "more than just").
- Generic, low-specificity elaboration — many words, few concrete, checkable
  particulars (this often overlaps with, and is better reported as, an
  *evidence/attribution* finding in the main audit).
- Overly even register with no idiolect, typos, or personal detail across a long
  passage.
- Vocabulary tics that co-occur with generated text — but treat any single tic as
  near-worthless on its own.

None of these individually means much. Only a *cluster* of them, in a longer
passage, moves the confidence band above "low", and even then it stays a signal.

## Confidence bands

Report exactly one, with the markers that justify it:

- **Inconclusive** — default. Too short, mixed, or too few markers to say anything.
- **Low** — a couple of markers; easily explained by ordinary careful/edited
  human writing.
- **Moderate** — a consistent cluster of markers across a substantial passage.
  Still not proof; still reversible.

Do **not** offer a "high"/"certain" band. The tool is not calibrated to earn it,
and asserting near-certainty about authorship is exactly the failure mode to avoid.

## Report shape

In the flagged item's advisory section:

```
## AI-style signal (advisory — does not affect scores or verdicts)
- Item: [which flagged item]
- Confidence: [Inconclusive / Low / Moderate]
- Markers observed: [marker — where it occurs (locator); marker — where; …]
- Caveat: These are weak stylistic signals, not proof of authorship. This does
  not change any claim's verdict or the audit scores, and makes no assertion
  about who wrote the text.
```

If the honest read is Inconclusive, still say so in one line rather than dropping
the section silently on a flagged item — the reader should see the pass ran and
found little.
