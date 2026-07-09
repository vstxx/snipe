# Report Format

Two templates: the **single-item report** (Article Snipe, and any single-item
Adaptive run) and the **multi-source report** (Site/Feed, Company, Person, and
multi-source Adaptive). Both lead with findings; the reader wants to know what
holds up, not to be re-told what the source said. Keep prose tight and
audit-toned — precise, neutral, defensible. Every verdict and score must be
traceable to evidence stated in the report.

Scale the depth to the material: a short article may have 5 claims, a think-tank
report 40, a 100-item company run hundreds (of which you verify the top 5–10 in
detail). Don't pad, and don't drop material claims to keep it short.

---

## Single-item template

```
# GrowGoSnipe Audit — [Document title]

## Document metadata
- Source: [title, outlet/publisher]
- Author: [name(s) or "unattributed"]
- Date: [publication date, or "undated — flag"]
- Type: [news / op-ed / think-tank report / policy brief / corporate / academic]
- Orientation noted: [known leaning of author/publisher, as context only]

## Verdict at a glance
- Claims audited: [n]  (High-materiality: [n], of which verified: [n])
- Supported: [n] · Mostly supported: [n] · Mixed: [n] · Misleading: [n] ·
  Unsupported: [n] · Contradicted: [n] · Outdated: [n] · Unverifiable: [n] ·
  Deferred: [n]
- Scores: Evidence quality [/100] · Claim accuracy [/100] ·
  Attribution strength [/100] · Overall integrity [/100]
- **Composite GrowGoSnipe Score: [/100]**  [one line: weights used + any override]
- Bottom line: [2–3 sentences. What holds up, what doesn't, and the single most
  important finding — e.g. "Figures are accurate but framed to mislead", or
  "Central statistic is contradicted by the primary source".]

## Key findings
[3–6 bullets. The most consequential results only: contradicted load-bearing
claims, serious framing problems, the strongest and weakest points. This is the
section a busy reader stops at.]

## Goal readout  [only if an audit goal was given — interpretation, not a verdict or score]
- Goal: [the objective, in operational terms]
- Answer for the goal: [what the audited evidence supports for this decision,
  resting only on the verdicts below; say so if the audit can't answer it]
- Load-bearing for the goal: [the goal-critical claims and their verdicts]
- Counter-goal findings: [findings that cut against the goal — surfaced first,
  not buried; "none material" only if true]
- What would change the answer: [unresolved claims/evidence that would flip it]
- Before acting: [what to verify first — points into Manual review recommended]
- Note: the goal shaped priority and presentation, not any verdict or score.

## Claim-by-claim findings
[One entry per material claim. Order by materiality, high first.]

### C1 — [claim restated neutrally in your own words]
- Type: [statistical / causal / attribution / …]
- Claim-maker: [article/editorial voice / quoted person / referenced report /
  named institution / unnamed experts / unclear — name the source where applicable]
- Source: [hard link to where the claim was made + locator — e.g. a
  `#:~:text=` deep link to the sentence, or "¶4 / section 'Results'", or a video
  timestamp. For pasted text with no URL: locator only. This is origin, not
  evidence.]
- Materiality: [High / Medium / Low]
- Verdict: [label from verdict-rules.md]
- Flag integrity check: [only if verdict is Contradicted, or an Integrity flag
  applies — passed / corrected (state the downgrade) / could not confirm. See
  flag-integrity-check.md. Omit this line for all other verdicts.]
- Evidence: [what you found, and the source(s) relied on — name the primary
  producer. Keep any quotation short and attributed.]
- Note: [scope, definition, date, or attribution caveats; omit if none]

### C2 — …
[…]

[Lower-materiality claims may be grouped in a compact table rather than given
full cards, as long as each still shows verdict and evidence.]

## Framing and integrity flags
[Document-level findings from the integrity audit. Cite the specific spot in the
text for each. Omit the section only if genuinely none apply — say so explicitly
rather than leaving it blank.]
- [Flag type]: [what triggered it and why it matters]

## Coverage and limits
- [What could not be verified and why — Unsupported vs. Unverifiable vs.
  out-of-scope.]
- [Searches attempted for the notable gaps, so the reader sees the gap is in the
  evidence, not the effort.]

## Manual review recommended
[List the specific claims a human should verify before relying on this audit.
Include: high-materiality claims rated Unsupported or Unverifiable; claims where
sources conflict and the call was close; quotes traced only to a single outlet;
anything resting on a Tier-3/Tier-4 or interested source; and claims where your
own confidence is low. For each, say in a few words WHY it needs human eyes and
WHERE to look. If nothing needs manual review, say so explicitly.]
- [C#]: [why it needs human verification, and the primary source to check]

## Sources consulted
[List the sources you actually relied on for verdicts, grouped roughly by tier,
so the audit is checkable. Links where available.]
```

---

## Notes on using the template

- **Restate claims neutrally.** In the claim cards, strip the rhetoric — audit
  the factual core, not the editorializing.
- **Name primary sources in evidence.** "Confirmed against Eurostat's harmonized
  series" beats "confirmed online". The reader should be able to re-run your
  check.
- **Keep verdict and framing separate.** Individual-claim verdicts go in the
  claim cards; document-level manipulation goes in framing flags. A true claim
  with a misleading presentation appears as Supported in its card and as a
  cherry-picking/framing flag in the flags section.
- **Don't bury the lede.** If one central figure is contradicted, that belongs
  in the bottom line and key findings, not only in card C17.
- **Format for the medium.** In chat, render the report as clean markdown
  inline. If the user asks for a file (docx/pdf), produce it with the relevant
  document skill; otherwise keep it in-conversation.
- **Match length to the text.** Resist both padding a thin article into a long
  report and compressing a dense report so much that material claims vanish.

---

## Multi-source template

For Site/Feed, Company, Person, and multi-source Adaptive runs. Use this exact
section structure. It is findings-first: the scope and source map orient the
reader, but the top claim risks and detailed verification are the point.

```
# GrowGoSnipe Multi-Source Report — [target]

## 1. Scope
- Mode: [Site / Company / Person / Adaptive]
- Target: [name / URL]
- Goal: [the audit goal in operational terms, or "none — neutral audit"]
- Window / limit: [for Company & Person: the trailing time window used, default
  "last 4 months (from [date] to today)"; for Site/Feed: item count. State the
  safety cap if one was hit, and note items with unknown dates that couldn't be
  confirmed inside the window.]
- Collected items: [n]
- Deduplicated items: [n]
- Sources used: [brief]
- Date range: [earliest → latest published_at]
- Focus: [claim focus, or "all checkable claims"]
- Limitations: [what couldn't be collected/accessed; connector caveats]

## 2. Source map
[Only the categories that apply.]
- Official website:
- Newsroom / blog:
- Press releases:
- LinkedIn:
- X/Twitter:
- YouTube:
- Reports / docs / filings:
- Reputable media / interviews:
- Other sources:

## 3. Source manifest summary
| Source type | Items | Text available | Claim count | Preliminary risk |
|---|---:|---:|---:|---:|

## 4. Item-level risk table
| Item | Source | Date | Claim count | Risk | Main issue |
|---|---|---|---:|---:|---|

## 5. Top claim risks
| Claim ID | Claim | Claim maker | Source item | Type | Verdict | Materiality | Risk |
|---|---|---|---|---|---|---:|---:|

## 6. Detailed verification
[Top 5–10 highest-risk / highest-materiality claims, unless exhaustive analysis
was requested. For each:]
- Claim:
- Original sentence:
- Claim maker:
- Source item:
- Source link + locator: [hard link to origin + where inside it — deep link,
  paragraph, timestamp, or permalink; origin, not evidence]
- Evidence needed:
- Flag integrity check: [only if the resulting verdict is Contradicted, or an
  Integrity flag applies — passed / corrected / could not confirm. Omit otherwise.]
- Evidence found:
- Assessment:
- Verdict:
- Confidence:
- Notes:

## 7. Aggregate pattern analysis
[Patterns across items:] recurring unsupported claims · repeated marketing
claims · vague authority language · missing dates · weak source chains · strong
primary evidence · contradictions · framing risk · attribution risk · source
transparency.

## 7b. Representative roster  [Company Snipe only]
[Who was auto-included as a company representative, and who was excluded and why.
Keep it bounded and transparent — this is the audited network, and the reader must
see its edges.]
| Person | Role | Category | Sources covered | Items in window | Included? |
|---|---|---|---|---:|---|
[Category = board / C-suite / official spokesperson / brand ambassador. Excluded
rows show the reason: out of scope, no public statements in window, cap reached,
identity unconfirmed, privacy.]

## 7c. Employee sample  [Company Snipe only]
[Broader than the representative roster: ordinary employees who publicly
identify as working at the company, collected via the strict dual-source rule
(Anysite + web_search/web_fetch, always both — see anysite-mcp-usage.md).
State the sampling basis plainly — this is a sample, not a census.]
| Person (or handle) | Role (if known) | Platform | Identified via | Items in window |
|---|---|---|---|---:|
[Sampling basis: N employees by visibility/engagement within the window, or
however the sample was actually built. Note the cap and that unnamed/unverifiable
accounts claiming employment were excluded unless corroborated.]

## 7d. Cross-source consistency — company vs. representatives vs. employees  [Company Snipe only]
[The payoff of the network audit: does what representatives and employees say
publicly match the company's official line? One row per claim that appears on
multiple sides or on one side conspicuously.]
| Claim | Company line | Rep/employee line (who, which layer) | Status | Note |
|---|---|---|---|---|
[Status = consistent / conflict / rep-or-employee-only-unconfirmed /
company-only / timing-mismatch. Flag conflicts and unconfirmed claims as
attribution risk; a representative or employee asserting something the company
hasn't confirmed is a finding, not a confirmation.]

## 7e. Recurring / patterned claims  [all multi-source modes]
[Claims that recur near-verbatim across different items or people in this run.
A cluster of matching phrasing across authors is a shared-script signal, not
independent corroboration — surface it, don't silently fold it into a single
"confirmed" claim.]
| Claim pattern | Appears in (items/people) | Verbatim or near-verbatim? | Read |
|---|---|---|---|
[Read = likely shared source/script / likely independent coincidence / unclear.
Cross-reference duplicate_group from the manifest (§4 of collection-workflow.md)
where relevant — this table is the whole-run view of the same discipline.]

## 7f. Goal readout  [only if an audit goal was given — interpretation, not a verdict or score]
- Goal: [operational restatement]
- Answer for the goal: [what the verified material supports for this decision;
  say so if it can't be answered]
- Load-bearing for the goal: [goal-critical claims/items and their verdicts]
- Counter-goal findings: [findings against the goal — surfaced prominently]
- What would change the answer: [unresolved evidence that would flip it]
- Before acting: [points into §8 Manual review]
- Note: the goal shaped priority and presentation, not any verdict or score.

## 7g. Comparative scorecard  [Company Snipe by default; strict fixed pattern — see comparative-scorecard.md]
[The standardized, cross-audit-comparable summary. Fixed definitions so separate
runs line up.]
- Sector: [how the company was classified, and on what basis]
- Adverse rate: [X% — (Misleading + Contradicted) / Assessed], with counts:
  [n Misleading, n Contradicted, of N assessed]
- Excluded from denominator: [n Deferred, n Unverifiable]
- Secondary: Unsupported rate [X%] · Coverage [Assessed/Extracted = X%]
- Band: [Clean / Low concern / Elevated / High / Severe — on GrowGoSnipe's fixed
  convention, not a measured sector norm]
- Sector positioning: [ONLY from a real basis — peer audits on file (state how
  many) or cited external data (cite it). If neither exists: "no sector baseline
  available — this is a baseline datapoint." Never invent a sector average.]
- Standardized summary line emitted to
  `grow-go-snipe-scorecard-[target]-[date].csv` (the unit of cross-audit
  comparison — collect these to build the sector benchmark).

## 8. Manual review recommended
[Claims/items a human should verify, each with why and where to look.]

## 9. Scoring
- Evidence Quality: [/100]
- Claim Accuracy: [/100]
- Attribution Strength: [/100]
- Overall Integrity: [/100]
- Composite GrowGoSnipe Score: [/100]  [weights + any override]
- Final judgement: [2–3 sentences: what the body of material shows overall.]
```

### Notes on the multi-source template
- **Manifest is working state, not output.** Print the summary table (§3), not
  the full manifest. Surface individual items through the risk tables and the
  detailed-verification cards. For small runs (≈≤25 items) the §3 summary and §4
  item-level risk table may stand in for the structured manifest entirely; for
  large runs, still keep the full per-item manifest as working state behind them
  (see source-manifest-schema.md).
- **Provenance everywhere.** Every claim in §5/§6 names its source item; every
  item traces to a source in §2.
- **Aggregate is the payoff.** §7 is what a single-item audit can't produce — the
  cross-item patterns (same unsupported stat reused, superlatives with no
  backing). Give it real weight.
- **Scores summarize the verified set**, weighted by materiality, with coverage
  stated (how much of the collected material was actually verified) so a clean
  score on thin coverage isn't mistaken for a clean bill. Same scoring rules and
  composite as the single-item report (scoring.md).
- **Privacy in the output too.** Don't surface private data even if a source
  exposed it, and for Person Snipe keep everything on public/professional claims.

---

## Standard deliverables (all modes)

Two things ship with **every** run, single-item or multi-source, in addition to
the in-chat report.

### A. Raw claim CSV — the complete claim ledger

The in-chat report shows only the material / top claims in detail. The CSV is the
**full superset**: one row per *extracted* claim — verified or not, high or low
materiality, nothing dropped. It is the raw, machine-readable ledger a human or a
downstream tool can re-audit.

- **Always produce it.** Even a 5-claim Article Snipe gets a CSV. Write it to
  `/mnt/user-data/outputs/` and present it at the end of the response (after the
  report), so the report reads clean and the file is there for anyone who wants
  the full list. Name it `grow-go-snipe-claims-[target]-[YYYY-MM-DD].csv`.
- **Columns** (in this order):
  `claim_id, item_id, claim_source_url, claim_source_locator, claim_text,
  original_sentence, claim_maker, type, entities, date_scope, materiality, risk,
  verdict, flag_integrity_check, confidence, evidence, evidence_urls`.
  For single-item runs `item_id` is constant and `claim_source_url` is the one
  document; still fill `claim_source_locator` per claim. `flag_integrity_check`
  is `passed`/`corrected`/`could not confirm` only where it ran (Contradicted
  verdicts and Integrity flags — see flag-integrity-check.md); leave it blank
  for every other row rather than filling it with a default.
- **Verdict/confidence/evidence** are filled for the claims that were verified and
  left as `not verified` (not blank, not guessed) for the ones that weren't — the
  CSV must make coverage honest, not hide it.
- **Copyright guard.** `original_sentence` is a *short* snippet only (well under a
  sentence), never the full paragraph — across hundreds of rows, full sentences
  would accumulate into substantial reproduction of the source. When in doubt,
  shorten it or lean on `claim_source_locator` instead.
- The CSV is the raw ledger; the report is the curated audit. Say in one line in
  the report that the full claim list is attached as CSV, so no one mistakes the
  report's top-claims view for the whole set.
- If the user explicitly prefers a spreadsheet with filters, produce `.xlsx`
  instead via the xlsx skill; CSV is the default.

### C. Comparative scorecard record — cross-audit comparability (Company Snipe)

For Company Snipe (and multi-source runs on request), also emit the standardized
scorecard summary line to
`/mnt/user-data/outputs/grow-go-snipe-scorecard-[target]-[date].csv` with the
fixed fields in `references/comparative-scorecard.md`. This one row is the unit
that makes separate audits comparable; the adverse rate and sector positioning in
report §7g are computed by its strict rules, and sector positioning is drawn only
from a real basis, never invented.

### B. AI-style signal — advisory, isolated, opt-in-by-trigger

For flagged posts/items only (those carrying a high-risk / flagged claim), run the
AI-style pass from `references/ai-style-signal.md` and report it in a **clearly
separated, explicitly hedged** section — never inline with the factual verdicts.

- Single-item: add a `## AI-style signal (advisory)` section *after* the framing
  flags and *before* scoring, only if the item was flagged; otherwise omit.
- Multi-source: add it as an advisory sub-section under the item-level findings /
  aggregate analysis, listing only the flagged items it ran on.
- **It never enters the four axis scores or the composite.** A claim's truth is
  independent of who or what wrote the post. Keep the signal out of scoring
  entirely and say so.
- **Never a binary "this is AI-written" verdict, and never an accusation against
  a named real person.** Report observed stylistic markers, where they occur, and
  a calibrated confidence band, with the standing caveat that these are weak
  signals, not proof of authorship. Full rules in `ai-style-signal.md`.
