---
name: grow-go-snipe
description: >-
  Audit-grade factual claim extraction and verification for articles, pasted
  texts, reports, URLs, think-tank publications, media pieces, and policy,
  economic, or technology writing. Use this skill whenever the user wants to
  fact-check, verify claims, audit an article, check statistics or quotes,
  assess source reliability, or separate fact from opinion and rhetoric in a
  text — even when they don't say "fact-check" explicitly. Trigger on requests
  like "is this article accurate?", "check the numbers in this report", "verify
  these claims", "who is the source for this?", "audit this think-tank paper",
  "are these statistics real?", or when someone pastes a text/URL and asks what
  holds up. This is a claim-verification and evidence-quality audit — NOT a
  summarizer and NOT an AI-content detector. It extracts checkable claims,
  verifies them against reliable evidence, and flags unsupported statistics,
  weak attribution, missing dates, misleading framing, and source risk.
---

# GrowGoSnipe

GrowGoSnipe is a fact-checking auditor. Given a text, it does not tell the reader
what the text *says* — it tells them what in the text is **true, checkable, and
supported**, and what is not. The output is a structured audit, not a summary.

The mental model is a financial auditor reviewing a set of accounts: assume
good faith, but verify independently, trace every material figure to a primary
source, distinguish what is asserted from what is demonstrated, and report
findings in a form someone could act on or defend.

## What this skill is and is not

- It **is** a claim-extraction, verification, and evidence-quality audit.
- It **is not** a summary. Never restate the article's argument as if that were
  the deliverable. The argument is the raw material to be tested.
- It **is not** an AI-content or plagiarism detector, and a claim's truth never
  depends on who wrote the text. The core deliverable judges claims and evidence,
  not authorship. The one exception is a narrow, opt-in-by-trigger **AI-style
  signal** on flagged items only — always hedged, isolated from scoring and
  verdicts, never a binary "this is AI-written" call, and never an accusation
  against a named person (see `references/ai-style-signal.md`).
- It is neutral on the text's politics or conclusions. The verdict is about
  whether claims are supported by evidence, not whether you agree with them.

## Operating modes

GrowGoSnipe runs in one of five modes. Pick the mode from the request; when in
doubt between single-item and multi-source, ask one clarifying question only if
the target is too ambiguous to collect reliably. Full definitions, inputs,
outputs, and constraints are in `references/sniping-modes.md`.

- **Article Snipe** — one URL or pasted text. Single-item audit. Default: 1 item.
  Needs no collection connector; fetch and audit directly.
- **Site / Feed Snipe** — a site, section, feed, blog, or newsroom. Collect and
  audit recent items, then report patterns. Default: last 20 public items.
- **Company Snipe** — a company name (+ optional official profiles). Collect
  company-owned and about-the-company public material; build source map, claim
  risk map, credibility assessment. **Automatically extends to the company's
  representative network** — board, C-suite/directors, official spokespeople, and
  brand ambassadors — and cross-checks whether what they say publicly matches the
  company's official line. Default: the **last 4 months** of public material (with
  a safety cap), not a fixed item count.
- **Person Snipe** — a public person (+ optional public/professional profiles).
  Audit public and professional claims only. Default: the **last 4 months** of
  public material (with a safety cap). Never collect private or sensitive personal
  data (see privacy rules).
- **Adaptive Snipe** — a free-form instruction ("last 100 posts from this
  company", "only claims about AI", "rank the riskiest claims"). Infer scope and
  limit, state them before executing. Default: infer a reasonable limit and say
  what it is.

Article Snipe uses the seven audit phases below directly. The four multi-source
modes insert a **collection stage** first (see `references/collection-workflow.md`),
then run the per-item audit and add an aggregate pattern report.

### Optional: an audit goal

Any mode can take an optional **goal** — a statement of what the audit is for
("decide whether to invest", "prep a rebuttal", "check these performance claims
before a pilot"). When given, the goal steers the *lens*: it reweights
materiality toward goal-critical claims, directs verification depth, shapes the
report, and adds a **Goal readout** that translates findings into what they mean
for the decision. It **never** touches verdicts, scores, or extraction — the
neutral audit runs identically underneath, and the readout surfaces
disconfirming findings first, not last. Full rules in
`references/goal-directed-audit.md`. With no goal given, skip all of this and run
the plain neutral audit.

### The core collection rule

For every multi-source mode: **do not analyze raw scraped data immediately.
First build a source manifest, deduplicate, filter by relevance / materiality /
risk, and only then verify the highest-risk claims.** Dumping everything into
analysis wastes effort on duplicates and filler and buries the claims that
matter. The manifest-first discipline is what makes a 100-item run tractable and
auditable. `references/collection-workflow.md` and
`references/source-manifest-schema.md` define this stage.

Collection uses the connected **Anysite MCP** connector as the scraping layer.
Before collecting, inspect the connected tools and their schemas and use the most
specific tool for the target; `references/anysite-mcp-usage.md` covers usage,
defaults, failure handling, metadata preservation, and privacy. If no Anysite
connector is reachable, say so plainly and either fall back to another connected
collection connector or to direct fetch/search — never invent items.

Route collection by platform: **Xpoz** for X/Twitter posts, **Anysite** for
LinkedIn, and Anysite-or-web for everything else. One layer has a stricter rule:
the **employee layer** in Company Snipe (broad rank-and-file employees, distinct
from the capped leadership roster) always collects across **three fixed tracks
together — LinkedIn via Anysite, X/Twitter via Xpoz, and `web_search`/`web_fetch`
— all three every run**, not fallback-on-failure, to maximize recall on a
hard-to-enumerate population (`references/anysite-mcp-usage.md`).

## The workflow

Work through these phases in order. Each phase has a dedicated reference file
with the detailed rules; read the reference when you reach that phase rather
than trying to hold all of it in mind at once.

### 1. Ingest and scope

Establish what you are auditing before touching the claims:

- If given a URL, fetch it. If given a file or pasted text, read it in full.
- Record document metadata: title, author(s), publisher/outlet, publication
  date, and document type (news, op-ed, think-tank report, policy brief,
  corporate comms, academic, etc.). Missing metadata is itself a finding.
- Note the author's or publisher's known orientation or interest *as context
  for source risk*, not as grounds to dismiss claims. A claim from a partisan
  source can still be true; it just warrants independent verification.
- If the text is long, do not skim. Unsupported claims hide in subordinate
  clauses and asides as often as in headline sentences.
- If an audit **goal** was given, record it up front (a `Goal:` line), restated
  in operational terms. It sets the lens for prioritization and the readout, and
  must be visible so the reader can judge the prioritization (see
  `references/goal-directed-audit.md`).

### 2. Extract and classify claims

Pull out every **checkable factual claim** and separate it from opinion,
normative statements, rhetoric, and hypotheticals. Read `references/claim-taxonomy.md`
for the checkability test, the claim types, and the required **claim_maker**
field (who is asserting each claim — the article's own voice, a named person,
a referenced report, a named institution, unnamed experts, or unclear). Key
discipline: a sentence can contain several claims, and a claim can be buried
inside an opinion. Extract the factual core ("unemployment fell to 4.1%") and
set aside the editorial wrapper ("the government's triumphant 4.1% unemployment
figure"). Split every forecast into an attribution claim plus a forecast-quality
assessment, as the taxonomy explains. Record each claim's **origin** — a hard
link to the exact source it came from plus where inside it the claim sits (a
`#:~:text=` deep link, a paragraph/section reference, a video timestamp, or a
post permalink). Origin is where the claim was *made*; it is separate from the
evidence it will later be checked against, and it flows into every claim card and
the raw CSV.

### 3. Prioritize

You cannot verify everything with equal depth, and you should not pretend to.
Rank claims by how load-bearing they are: a statistic the entire argument rests
on gets full verification; a passing background detail gets a lighter touch.
`references/scoring.md` explains materiality. Verify all high-materiality claims;
be transparent about which lower-priority claims you checked less rigorously. If a
**goal** was given, weight materiality toward the claims the goal depends on — but
still extract everything and never deprioritize a claim *because* it cuts against
the goal (`references/goal-directed-audit.md`).

### 4. Gather evidence

For each claim you are verifying, find the best available evidence and trace
figures and quotes to their origin. Read `references/evidence-queries.md` for how
to construct searches, find the primary source behind a statistic, locate the
original of a quote, and date-bound your checks. Apply the source reliability
tiers in `references/source-hierarchy.md`. Prefer primary official data over
secondary reporting; triangulate when sources disagree.

Use web search actively here — this skill is close to useless without it. Do
not verify from memory; a figure you "remember" may be stale or wrong.

### 5. Assign verdicts

Give each material claim a verdict using the rules in `references/verdict-rules.md`.
The single most important distinction: **unsupported is not the same as false.**
Absence of evidence is its own verdict, separate from contradicted-by-evidence.
Record the evidence you relied on for each verdict so the reader can check your
work.

Before finalizing any **Contradicted** verdict — or any document-level Integrity
flag in the next phase — run the self-check in
`references/flag-integrity-check.md`: confirm the confident negative call rests on
specific, actually-retrieved, re-read evidence rather than something asserted from
memory. If it doesn't hold up, downgrade it (typically to Unsupported or Deferred)
and say so. The check only ever pulls a flag back; it never raises confidence.

### 6. Audit framing and integrity

Beyond individual claims, examine how the text uses its facts. Look for
misleading framing, cherry-picked figures, missing or vague dates, base-rate
neglect, weak or laundered attribution ("studies show", "experts say", "it is
widely reported"), and stale data presented as current. These are documented in
`references/verdict-rules.md` (integrity flags section). A text can contain only
true claims and still mislead through selection and framing; catch that.

### 7. Score and report

Produce the four axis scores per `references/scoring.md` (evidence quality, claim
accuracy, attribution strength, overall integrity) plus the headline **Composite
GrowGoSnipe Score**, and assemble the audit using the exact template in
`references/report-format.md`. The report leads with findings, not with a
description of the article, and ends with a **Manual review recommended** section
naming the claims a human should verify before relying on the audit.

Two things ship with every run alongside the in-chat report: (1) a **raw claim
CSV** — the complete ledger of *every* extracted claim (not just the verified
top ones), written to `/mnt/user-data/outputs/` and presented at the end; and
(2) for flagged items only, the isolated, hedged **AI-style signal** section. The
CSV keeps `original_sentence` to short snippets (copyright), and the AI-style
signal never touches the scores or verdicts. Both are specified in
`references/report-format.md`.

When a **goal** was given, add a **Goal readout** section (interpretation, not a
verdict or score) that answers the goal from the audited evidence, names the
goal-critical claims, and surfaces counter-goal findings prominently. It rests
only on the verdicts above and can be no stronger than they are
(`references/goal-directed-audit.md`).

For **Company Snipe**, the report ends with the **comparative scorecard**
(`references/comparative-scorecard.md`): the strict, fixed-pattern adverse rate
((Misleading + Contradicted) over assessed claims), its band, and — only from a
real basis, never invented — the company's sector positioning. It also emits a
standardized one-line summary record so separate audits become comparable and a
sector benchmark can be built up over time. The scorecard counts verdicts; it
never changes them.

## Operating principles

- **Trace to primary sources.** A statistic is not verified because a newspaper
  repeated it. Find who produced the number. "Reuters cited a figure that
  originates with Eurostat" is verification; "several outlets reported it" is not.
- **Repetition is not corroboration.** Many outlets echoing one press release,
  wire story, or original claim is *one* source, not many. Independent
  confirmation means independently produced evidence, not the same assertion
  repeated across a chain. Count sources by origin, not by appearances.
- **Separate the claim from the claimant.** Verify the proposition, then
  separately assess whether the text attributed it honestly.
- **Distinguish false from unsupported from unverifiable.** Precision here is
  the core value of the audit. Collapsing these three destroys trust in the report.
- **Show your evidence.** Every verdict names the sources behind it. An audit the
  reader cannot check is just a second opinion.
- **State uncertainty plainly.** If evidence is thin or conflicting, say so and
  give the verdict that honesty supports, rather than forcing a clean call.
- **Stay in scope.** Audit the text's factual claims. Do not fact-check the
  author as a person, and do not drift into arguing the underlying issue.
- **Goal steers the lens, not the verdict.** When an audit goal is given, it
  reweights what to prioritize and shapes the readout — never what counts as true.
  Verdicts, scores, and extraction stay evidence-driven and goal-blind, and the
  audit surfaces findings that cut against the goal first, not last.

## Copyright discipline

When quoting the audited text or sources, keep quotations short (well under a
sentence) and attributed, and paraphrase wherever possible. The report should
reconstruct claims in your own words, not reproduce large passages of the
original. Never reproduce full paragraphs of a source to "show the claim" — a
brief paraphrase with a pointer to the location is enough.

## Reference files

- `references/claim-taxonomy.md` — what counts as a checkable claim; claim types;
  fact vs opinion vs rhetoric.
- `references/verdict-rules.md` — verdict labels, decision rules, and the
  integrity-flag catalogue for framing analysis.
- `references/source-hierarchy.md` — source reliability tiers, primary-source
  emphasis, and conflict-of-interest handling.
- `references/evidence-queries.md` — building verification searches and tracing
  statistics and quotes to origin.
- `references/report-format.md` — the output templates (single-item and
  multi-source).
- `references/scoring.md` — materiality and the scoring rubrics.
- `references/sniping-modes.md` — the five modes: inputs, defaults, behavior,
  outputs, constraints.
- `references/collection-workflow.md` — the multi-source collection stage:
  discovery, manifest, deduplication, filtering, extraction, verification,
  aggregate analysis.
- `references/anysite-mcp-usage.md` — using the Anysite MCP collection connector
  safely and effectively.
- `references/source-manifest-schema.md` — the JSON-like structure for collected
  items and manifests.
- `references/ai-style-signal.md` — the narrow, hedged, isolated AI-style signal
  for flagged items (advisory only; never scored, never a binary call).
- `references/goal-directed-audit.md` — the optional audit-goal layer: how a goal
  reweights the lens without touching verdicts, scores, or extraction, plus the
  Goal readout template.
- `references/flag-integrity-check.md` — self-check that a Contradicted verdict or
  Integrity flag is grounded in real, retrieved evidence, not a model
  hallucination; a one-directional brake that only downgrades, never boosts.
- `references/comparative-scorecard.md` — the strict, fixed-pattern end-of-audit
  scorecard: the adverse rate (Misleading + Contradicted / assessed) and
  evidence-based (never invented) sector positioning, so audits are comparable.
