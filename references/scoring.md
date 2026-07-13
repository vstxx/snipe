# Scoring

Scores turn the audit into something comparable and scannable. They are a
summary of the evidence-based findings, never a substitute for them — a score
without the underlying verdicts and flags is not an audit. Always derive scores
from the claim verdicts and integrity flags already established; never assign a
score by overall impression.

## Materiality (used for prioritization, not a published score)

Before scoring, rate each claim's materiality, because it decides verification
depth and weights the scores.

- **High** — the argument depends on it; if it falls, the piece's central point
  falls. Verify fully.
- **Medium** — supports the argument but isn't load-bearing. Verify.
- **Low** — background, color, or aside. Light check; be transparent that it was
  checked lightly.

Weight the scores toward high-materiality claims. Ten correct trivia and one
false load-bearing statistic is not a healthy document.

When an audit **goal** is given, materiality is assessed relative to the goal as
well as the text: a claim the decision depends on is High even if peripheral to
the article's own argument. The goal may *raise* attention and depth on
goal-critical claims; it must never drop a claim from the ledger or deprioritize
one *because* it cuts against the goal (`references/goal-directed-audit.md`).

## The four scores

Report each on a 0–100 scale with a one-line justification. Bands: 85–100
strong, 70–84 adequate, 50–69 weak, below 50 poor.

### 1. Evidence quality (of the source text)
How well the text supports its own material claims: are figures sourced, are
sources named and primary, are dates present, are quotes attributed?

- High when material claims cite identifiable, primary, current sources.
- Low when the text leans on laundered attribution, undated figures, or
  interested single sources for load-bearing claims.

### 2. Claim accuracy
How the material claims fared against independent verification, weighted by
materiality.

- Supported / Mostly supported count as accurate.
- Mixed counts partially.
- Contradicted and Misleading count against.
- Unsupported, Unverifiable, and Deferred are excluded from the accuracy ratio
  but reported separately as a coverage gap — don't let unchecked claims silently
  inflate or deflate accuracy. State how many material claims could not be, or
  were not, checked. Deferred claims in particular signal thin coverage: a high
  accuracy score built on a handful of checks with many Deferred claims is not a
  clean bill, and the composite's thin-coverage cap should apply.

### 3. Attribution strength
How honestly and traceably the text attributes its claims and quotes: named vs.
anonymous sources, primary vs. paraphrased quotes, citations that check out vs.
citations that misrepresent what they cite.

### 4. Overall integrity
A holistic judgment combining the above with the framing audit. This is where
"every claim true but the framing misleads" shows up: such a document can score
high on claim accuracy yet low on overall integrity. Explicitly reflect the
integrity flags here. Do not compute it as a naive average — a single
load-bearing Contradicted claim or a serious cherry-picking flag should pull it
down more than arithmetic would.

## Composite GrowGoSnipe Score

After the four axis scores, report a single **Composite GrowGoSnipe Score** (0–100)
as the headline figure. It is a *weighted, judgment-adjusted* roll-up, not a
plain average — the axes measure different things and a naive mean hides the
failure modes that matter most.

Suggested starting weights, then adjust with judgment:

- Claim accuracy — 40%
- Overall integrity — 30%
- Evidence quality — 20%
- Attribution strength — 10%

Then apply overrides, because some failures should dominate arithmetic:

- A **Contradicted high-materiality claim** caps the composite well below the
  weighted average (a document whose central figure is false is not "adequate").
- A **serious framing/integrity flag** on a load-bearing point (cherry-picking,
  quote distortion, correlation-as-causation on the main claim) pulls the
  composite down regardless of how many minor claims checked out.
- **Thin coverage** (most material claims Unsupported/Unverifiable) caps the
  composite in the "weak" band — you cannot certify what you couldn't check.

State the weights and any override you applied in one line, so the composite is
reconstructable from the axis scores. The composite is a convenience for the
reader, never a replacement for the claim table and flags.

## Scoring discipline

- **Traceability.** Each score must be defensible from the claim table and flag
  list. If asked "why 62?", the answer is in the verdicts, not in vibes.
- **Don't reward the unverifiable.** Claims you couldn't check don't count as
  correct. Report coverage (how much of the material you could actually verify)
  alongside the scores so a high accuracy score on thin coverage isn't
  mistaken for a clean bill.
- **Separate the axes.** Keep evidence quality (how well the text sourced
  itself), claim accuracy (whether it was right), attribution strength (whether
  it credited honestly), and integrity (whether the whole misleads) distinct.
  Their divergence is often the most informative part of the report — e.g. high
  accuracy, low integrity is the signature of sophisticated spin.
- **Scores are goal-blind.** An audit goal never enters the four axes or the
  composite — they measure the text, not the decision. Compute them identically
  with or without a goal; the goal readout lives outside the scores.
