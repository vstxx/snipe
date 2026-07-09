# Evidence Queries

Verification lives or dies on search craft. This file covers how to turn a claim
into searches that reach primary sources, and how to trace statistics and quotes
to their origin. Verify with the web, not from memory — a remembered figure is
an unverified figure.

## General approach

For each material claim, run a small campaign rather than a single query:

1. **Locate the primary source** of the specific fact.
2. **Confirm the exact figure/wording** against that source.
3. **Check the date and scope** match what the text claims.
4. **Look for contradiction** — actively search for evidence the claim is wrong,
   not only evidence it's right. Confirmation-only searching produces false
   Supported verdicts.

## Tracing a statistic to origin

The task is to find who produced the number, not who repeated it.

- Start from the specifics: the metric, the value, the period, the geography.
  Example claim: "Polish unemployment fell to 5.0% in 2023." Search toward the
  producer: the national statistics office's labour data, or Eurostat's
  harmonized series, for that exact year and definition.
- Name the likely producer in the query. For official data, search the
  statistics office, central bank, or regulator directly rather than news
  aggregators.
- Watch the **definition**. Registered unemployment vs. LFS/BAEL unemployment
  are different series with different values; "the 5% figure" may be true under
  one definition and wrong under the other. Verify against the definition the
  text implies, and flag if the text is ambiguous.
- Reconcile **units and basis**: nominal vs. real, seasonally adjusted or not,
  annual average vs. point-in-time, absolute vs. per-capita. A "verified" number
  measured differently from the claim is not verification.

## Tracing a quote to origin

- Search a distinctive fragment of the quote in quotation marks to find the
  earliest/primary appearance.
- Reach the **verbatim primary record**: official transcript, full-text speech,
  the recording, or the speaker's own published statement — not a second
  outlet's paraphrase.
- Check two things separately: did the person say it (attribution accuracy), and
  is the surrounding context preserved (no clipping that flips the meaning).
- If the quote exists only in the audited outlet with no independent trace, that
  is an integrity flag (single-source / unverifiable quote), not a Supported
  verdict.

## Existence checks

For "a study/report/law found X":

- First confirm the source **exists** and is correctly identified — title,
  author, publishing body, year.
- Then confirm it **says what the text claims**. Texts routinely cite real
  studies for conclusions the studies don't support.
- "A Harvard study" / "a UN report" with no identifying detail: search for the
  specific study; if none can be identified, the existence claim is Unsupported
  and the vague attribution is an integrity flag.

## Date-bounding

- Anchor every time-sensitive check to the claim's period, and separately check
  whether **newer data supersedes** the figure (→ Outdated).
- Include the year in searches, and use the actual current year for "latest"
  queries so you don't retrieve stale "latest" results.
- For "recently" / "record high" / "now" with no date, verify against current
  data and flag the missing date regardless of the outcome.

## Search discipline

- One claim, several angles: try the producer directly, try the metric +
  period, and try a contradiction search. Reformulate rather than repeating a
  query that missed.
- Keep queries short and specific; lead with the distinctive terms (proper
  nouns, exact figures, agency names).
- Prefer official portals and original documents over aggregators; use
  aggregators and encyclopedias to *find* the primary citation, then go to it.
- Record, for each verdict, which sources you actually relied on, so the report
  is checkable.

## When you can't verify

- Distinguish "searched properly and found nothing" (→ Unsupported) from "can't
  be checked in principle" (→ Unverifiable) from "found contradicting evidence"
  (→ Contradicted). Your search effort is what separates the first from a lazy
  non-answer, so search genuinely before settling on Unsupported.
- Note the searches you ran when a claim resists verification, so the reader can
  see the gap is in the evidence, not in the effort.
