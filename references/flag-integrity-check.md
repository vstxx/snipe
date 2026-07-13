# Flag Integrity Check

This is a different thing from `ai-style-signal.md`. That file asks whether a
*post's text* looks machine-written. This file asks whether **GrowGoSnipe's own
confident negative call** — a `Contradicted` verdict or a document-level
Integrity flag — is actually grounded in real evidence, or is an artifact of the
model asserting something it didn't actually verify. Don't confuse the two; if
useful, think of this one as "checking the checker."

## Why this exists

A confident wrong flag is worse than no flag: it damages a real person's or
company's credibility on the audit's own authority. The failure mode this guards
against is specific — a verdict or flag that *sounds* well-evidenced but traces
back to something the model recalled or inferred rather than something it
actually retrieved and read.

## Trigger — when to run this

Run the check on every claim/finding that gets one of these confident negative
calls, before it's finalized in the report:

- Any claim assigned the **Contradicted** verdict.
- Any **Integrity flag** applied to the document as a whole (verdict-rules.md).

Do **not** run it on Supported, Unsupported, Deferred, or Unverifiable — those
already carry their own honesty built in (Unsupported/Deferred openly say "no
evidence found" or "not checked yet", which is a much lower hallucination-risk
shape than a confident refutation). The check exists specifically for the calls
strong enough to damage someone if wrong.

## The check

For each triggered flag, walk through and record the result:

1. **Is there a specific, re-locatable piece of evidence behind this?** A URL,
   document, or retrieved passage — not a general impression. If you can't point
   to the specific thing that grounds the flag, that alone is disqualifying.
2. **Was that evidence actually retrieved this run** (a real search/fetch you
   performed), or is it being recalled from training/memory? A flag resting on
   unretrieved recall is high hallucination-risk by default — it must be
   re-confirmed with a fresh search/fetch before it can stand as Contradicted or
   as an Integrity flag. If it can't be re-confirmed, downgrade it now.
3. **Re-read the exact source text.** Does it actually say what the flag claims,
   or is this a misreading — a sign flip, a similar-sounding entity conflated
   with the real one, a number attached to the wrong year/scope, a quote taken
   out of context? Reread before trusting your first pass.
4. **Would this hold up cold?** If you re-derived the verdict from the same
   evidence with no memory of your first pass, would you land on the same call?
   If you're not confident the answer is yes, that's a signal to downgrade.

## Outcome

Record one of three outcomes against the flag, and act on it:

- **Passed** — evidence is specific, retrieved, re-read, and holds. The flag
  stands as-is.
- **Corrected** — the check surfaced a problem. Downgrade the verdict (typically
  `Contradicted` → `Unsupported` or `Deferred`; an Integrity flag → removed or
  softened to a note) and say so plainly rather than quietly editing it away.
  A corrected flag is a normal, healthy output of this check — report it, don't
  hide the correction.
- **Could not confirm** — you couldn't complete the check (evidence no longer
  accessible, ambiguous re-read). Downgrade to `Deferred` and put it in Manual
  review rather than leaving an unconfirmed confident call standing.

Never let this check *raise* confidence — it only holds a flag steady or pulls it
back. It is a one-directional brake, not a booster.

## Reporting

Add a short `Flag integrity check` line only where the check ran (i.e. only next
to Contradicted verdicts and Integrity flags) — in the single-item claim card, the
multi-source verification card, and the `flag_integrity_check` column in the raw
CSV (`passed` / `corrected` / `could not confirm`). Omit the line entirely for
claims that never triggered the check; don't clutter every claim with it.
