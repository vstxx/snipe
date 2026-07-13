# Verdict Rules

This file governs two things: the verdict assigned to each individual claim, and
the integrity flags applied to the text as a whole. Keep them separate — a
document made entirely of true claims can still earn serious integrity flags.

## Verdict labels

Use exactly these labels. The precision of the ladder is the point; do not
invent intermediate labels or blur them.

| Verdict | Meaning |
|---|---|
| **Supported** | Independent, reliable evidence confirms the claim as stated. |
| **Mostly supported** | Confirmed in substance; a minor detail (rounding, date precision, phrasing) is off but doesn't change the meaning. |
| **Mixed** | Partly confirmed, partly not; the claim bundles a true and a false or unverified element. |
| **Misleading** | The literal words are defensible but the claim creates a false impression through framing, selection, or missing context. Technically-true-but-deceptive lives here. |
| **Unsupported** | No reliable evidence found either way. NOT a synonym for false. The claim may be true but is presented without support and none could be located. |
| **Contradicted** | Reliable evidence directly refutes the claim as stated. Before finalizing this verdict, run the self-check in `references/flag-integrity-check.md` — confirm the refutation is grounded in specific, actually-retrieved, re-read evidence, not an unverified impression. |
| **Outdated** | Was accurate at some point but reliable current evidence supersedes it. |
| **Unverifiable** | Cannot be checked in principle (private conversations, internal states, undisclosed data) or not within reasonable scope. Distinct from unsupported — here, checking is impossible, not merely unsuccessful. |
| **Deferred** | *Attributed — not verified this pass.* The claim is checkable and was left unchecked on purpose because prioritization put verification budget elsewhere. This is a status, not a judgement: it says nothing about whether the claim is true, only that the audit didn't test it yet. Every Deferred claim belongs in "Manual review recommended." |

Use **Deferred** instead of stretching "Mostly supported" or "Unsupported" to cover claims you simply didn't check. Those two labels are earned only after you look: "Mostly supported" means you verified and found a minor detail off; "Unsupported" means you searched properly and found nothing. A claim you deprioritized is neither — it is Deferred. Collapsing "I checked and it's fine-ish" together with "I didn't check" is exactly the blurring the verdict ladder exists to prevent, so keep Deferred visibly separate and never let it stand in for a real verdict on a high-materiality claim (verify those).

## The three distinctions that matter most

Most of GrowGoSnipe's value is in not collapsing these:

1. **False vs. unsupported.** "Contradicted" means evidence says no.
   "Unsupported" means evidence says nothing. Reporting an unsupported claim as
   false is itself a factual error, and destroys the audit's credibility. When
   you cannot find evidence, say exactly that.

2. **Unsupported vs. unverifiable.** "Unsupported" implies checkable-in-principle
   but unconfirmed in practice. "Unverifiable" implies no evidence could settle
   it. Don't punish a claim as unsupported when it was never checkable to begin
   with; flag the text for making an unverifiable assertion instead.

3. **False claim vs. misleading framing.** A precise figure can be individually
   true yet assembled into a misleading picture. Verdict the claim on its own
   terms, then capture the framing problem as an integrity flag.

## Decision rules

- **Burden of proof sits with the claim.** A specific factual assertion needs
  support to be rated Supported. Absence of support caps it at Unsupported, not
  Supported-by-default.
- **Match evidence strength to claim strength.** A strong causal or dramatic
  statistical claim needs strong evidence; a mundane background fact needs less.
  A single blog post does not verify an extraordinary claim.
- **When sources conflict,** weigh by the source hierarchy, prefer the primary
  producer of the data, check recency, and if a genuine dispute remains, rate
  Mixed and describe the disagreement rather than picking a side arbitrarily.
- **Quotes get a compound verdict.** State separately whether the attribution is
  accurate and whether the quoted claim is true. "Accurately quoted, but the
  quoted figure is contradicted" is a complete and common finding.
- **Forecasts** are split into two claims (see claim-taxonomy.md). The
  attribution half ("the central bank issued this forecast") gets a normal
  verdict — trace it to the forecaster's own publication. The forecast-quality
  half is never Supported/Contradicted on its content; rate its provenance and
  plausibility (credibility of forecaster, reasonableness of assumptions, fit
  with other serious projections, disclosure of uncertainty). Report the two
  separately.
- **Don't over-rate on thin evidence.** If you found one weak source, that is
  Unsupported or Mixed, not Supported. Reserve Supported for genuinely reliable
  confirmation.

## Integrity flags (document-level framing audit)

Apply these to the text as a whole, independent of individual verdicts. Each
flag should cite the specific spot in the text that triggered it. Before
finalizing any of these flags, run the self-check in
`references/flag-integrity-check.md` — the flag should trace to something
specific you actually retrieved and re-read, not an unverified impression.

- **Missing / vague dates** — figures or events with no date, or "recently",
  "last year" with no anchor. Undated statistics are a top-tier flag because
  they can't be checked and are often stale.
- **Stale data as current** — a real figure presented as if it reflects the
  present when newer data exists.
- **Weak / laundered attribution** — "studies show", "experts say", "it is
  widely believed", "sources report" with no identifiable source. Treat the
  underlying claim as effectively unsourced.
- **Cherry-picking** — a true figure chosen to mislead: a start date picked to
  flatter a trend, one metric highlighted while a contradicting one is omitted.
- **Base-rate neglect** — dramatic raw numbers with no denominator or context
  ("cases doubled" from 2 to 4).
- **Unit / scale manipulation** — nominal vs. real, absolute vs. per-capita,
  percentage vs. percentage-point, mismatched comparison periods.
- **False precision** — suspiciously exact figures implying a rigor the source
  doesn't have.
- **Anonymous or single-source load-bearing claims** — a central claim resting
  entirely on an un-nameable or lone source.
- **Correlation-as-causation** — causal language over what is only a correlation.
- **Source risk** — a load-bearing claim traced only to a source with a clear
  interest in it, or a Tier-3/Tier-4 source (see source-hierarchy.md).
- **Quote distortion** — a real quote clipped, stripped of context, or
  paraphrased in a way that changes its meaning.

A clean bill on individual claims plus several integrity flags is a valid and
important result: "the facts check out, but the picture they're arranged into is
misleading." Say that clearly when it's true.
