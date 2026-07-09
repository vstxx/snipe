# Goal-Directed Audit

A **goal** is an optional statement of what the audit is *for* — the decision it
feeds or the question it must answer: "decide whether to invest", "prep a
rebuttal to this piece", "check this vendor's performance claims before a pilot",
"monitor whether the competitor's public numbers hold up". Any mode can take one.

The goal makes GrowGoSnipe useful for a decision without compromising what makes
it trustworthy. The whole design turns on one line:

> **The goal steers the lens, never the verdict.**

If a goal is not given, none of this applies — run the neutral audit as usual and
omit the goal readout. The goal layer is purely additive.

## What the goal may control

- **Materiality weighting.** What is "load-bearing" is partly goal-relative. A
  claim peripheral to the text's own argument can be central to *your* decision,
  and vice versa. Let the goal *raise* attention on goal-critical claims.
- **Verification depth.** Spend the verification budget where the goal needs
  confidence. A claim irrelevant to the goal may get a lighter check — but it is
  still extracted and still on the ledger (see the hard limits below).
- **Report shaping.** The bottom line and key findings should speak to the goal's
  question, and the run ends with a **Goal readout** that translates the audit
  into what it means for the decision.
- **Manual-review focus.** The "before acting" list is framed around what the
  goal-holder must verify before relying on the audit for this decision.

## Hard limits — what the goal must never touch

1. **Verdicts.** Supported / Contradicted / Unsupported / … are decided by
   evidence only. The goal cannot nudge a verdict toward the answer you want.
2. **Scores.** The four axes and the Composite GrowGoSnipe Score measure the
   *text*, not your goal. They are computed identically with or without a goal.
   The goal readout sits outside the scores and says so.
3. **Extraction.** Extract every checkable claim regardless of the goal. The goal
   filters *priority and presentation*, never what gets pulled out. Filtering
   extraction by the goal is how you go blind to the claims that would change
   your mind — which is exactly what a decision-maker needs to see.
4. **The neutral audit underneath.** The full, goal-agnostic audit stands on its
   own. The readout is a layer on top, never a replacement.

## The anti-confirmation discipline (this is the "smart" part)

A naive goal layer becomes a confirmation machine: it looks only for what serves
the goal and quietly drops the rest. Do the opposite. The auditor's loyalty is to
the evidence, not to the goal. Concretely:

- Actively hunt for and **surface disconfirming findings prominently** — the
  readout has a dedicated *Counter-goal findings* line, and it is not optional.
- If the evidence disappoints the goal, the readout says so plainly and first.
  "The claims your case rests on are largely Unsupported" is the most valuable
  thing this layer can output.
- Never let a goal-favourable framing outrun the verdicts. The readout can be no
  stronger than the evidence it points to.

## Goal readout — template

Emit only when a goal was given. Place it as its own clearly-labelled section
(single-item: after Key findings; multi-source: after the aggregate/cross-source
sections, before Manual review).

```
## Goal readout  (interpretation — not a verdict or a score)
- Goal: [the objective, restated in operational terms]
- Answer for the goal: [what the audited evidence supports for THIS decision,
  2–4 sentences, resting only on the verdicts above. If the audit can't answer
  the goal, say that — don't manufacture an answer.]
- Load-bearing for the goal: [the claims/findings the decision most depends on,
  with their verdicts — the goal-critical subset]
- Counter-goal findings: [findings that cut against the goal or against the
  answer one would prefer — surfaced deliberately; "none material" only if true]
- What would change the answer: [the specific unresolved claims/evidence whose
  resolution would flip or firm up this readout]
- Before acting: [the manual-review items to verify first, pointing into the
  Manual review section]
- Note: the goal shaped what to prioritize and how to present — not any verdict
  or score. The neutral audit above stands on its own.
```

## Recording the goal

Record the goal in the scope/metadata up front (a `Goal:` line), so the reader
sees the lens the audit was run through and can judge whether the prioritization
was reasonable. A goal stated vaguely ("make the company look good") should be
restated in auditable, operational terms — and if a goal asks for advocacy rather
than assessment, hold the line: GrowGoSnipe assesses; it does not build a case by
suppressing contrary evidence.
