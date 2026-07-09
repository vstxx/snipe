# Sniping Modes

GrowGoSnipe runs in one of five modes. Every mode produces the same *kind* of
result — an evidence-quality audit of checkable claims — but differs in scope,
collection method, default limit, and report shape. Single-item mode audits
one text; the four multi-source modes collect many items first, then audit and
look for patterns across them.

Common to all modes: extract checkable claims, separate fact from opinion /
rhetoric / prediction / interpretation, verify the claims that matter against
reliable evidence, and flag unsupported statistics, weak attribution, missing
dates, misleading framing, and source risk. Never summarize as the deliverable.

On AI authorship: the deliverable is never a verdict that a text "is AI-written",
and the audit of a claim's truth never depends on who wrote it. For **flagged
items only**, an optional, clearly-separated **AI-style signal** (advisory,
hedged, isolated from scoring) may be reported per `references/ai-style-signal.md`
— observed stylistic markers with a confidence band, never a binary call and
never an accusation against a named real person.

Also common to all modes: an optional **audit goal** may be supplied. When
present it steers the lens (materiality weighting, verification depth, report
shaping, and a Goal readout) but never verdicts, scores, or extraction, and it
surfaces counter-goal findings first — see `references/goal-directed-audit.md`.

Two distinct integrity mechanisms exist and must not be confused. The **AI-style
signal** (`ai-style-signal.md`) asks whether a *post's text* looks
machine-written — advisory, flagged-items-only. The **flag integrity check**
(`flag-integrity-check.md`) asks whether *GrowGoSnipe's own* Contradicted verdict
or Integrity flag is grounded in real retrieved evidence rather than a model
hallucination — a self-check that only ever downgrades. One audits the source;
the other audits the auditor.

---

## 1. Article Snipe

**Input:** one URL, or pasted article text (article, page, report, press
release, blog post).

**Default limit:** 1 item.

**Behavior:** fetch or read the single item; run the seven audit phases directly
(ingest → extract & classify → prioritize → gather evidence → verdicts → framing
audit → score & report). No collection connector is needed — use direct fetch and
web search.

**Output:** the standard single-item GrowGoSnipe Report (see report-format.md).

**Constraints:** if a URL can't be fetched, say so; don't reconstruct the article
from memory or from its title.

---

## 2. Site / Feed Snipe

**Input:** a website, publication, section URL, RSS/feed, blog, newsroom, or
source page. Optional limit (last 10 / 20 / 50 / 100 items).

**Default limit:** last 20 public items.

**Behavior:** use the collection connector to discover and collect the most
recent public posts/articles; build a source manifest first; deduplicate by
canonical URL, title similarity, timestamp, author/account, and near-duplicate
text; extract metadata; classify each item; extract checkable claims per item;
prioritize high-risk / high-materiality claims; verify the top ones.

**Output:** the multi-source GrowGoSnipe Report — both item-level findings and an
aggregate pattern report.

**Constraints:** if fewer items exist than requested, say so. Follow the manifest
-first rule (collection-workflow.md).

---

## 3. Company Snipe

**Input:** a company name, plus optional official website, newsroom, blog,
LinkedIn, X/Twitter, YouTube, press page, product pages, investor relations,
docs, or other public profiles.

**Default limit:** the **last 4 months** of public material (trailing window from
today), not a fixed item count. Keep a safety cap (≈150 items after dedup) so a
prolific company doesn't blow up the run; if the cap is hit, say so and prioritize
by risk/materiality within the window. Items with unknown dates are kept but
flagged as unconfirmed-in-window. An explicit user limit or window overrides this.

**Behavior:** collect the last 4 months of public materials from and about the
company. Source priority: official / company-owned sources first, then official
social profiles, then official reports / docs / filings, then reputable interviews
and media, and finally lower-confidence third-party context. Extract claims about
products, customers, revenue, funding, market position, AI / technology, policy,
legal / regulatory issues, partnerships, timelines, performance, and any
"leading / first / biggest / record" superlatives. Build a company-level source
map, a claim risk map, and an aggregate credibility assessment.

**Representative network (automatic).** A company speaks through people, so a
Company Snipe automatically extends to the company's public representatives:
board, C-suite / directors, official spokespeople, and brand ambassadors /
representative figures, across the media where they post publicly. Do it bounded
and transparently:

- **Identify** representatives from official sources (leadership/about page,
  filings, official social, company press/marketing for ambassadors). Cap the
  roster (top representatives by seniority / public prominence) and print a
  **roster** of who was included and who was excluded and why (see report-format
  §7b).
- **Same window, per-person cap.** Each representative is collected over the same
  trailing 4-month window, with a per-person item cap so the roster doesn't
  dominate the run.
- **Cross-check.** Compare what representatives say publicly against the company's
  official line, and against each other. Surface conflicts, claims a representative
  asserts that the company hasn't officially confirmed (attribution risk), and
  timing mismatches (someone announced ahead of the official date). Report this as
  the cross-source consistency matrix (report-format §7d). A representative
  repeating the company line is *not* independent corroboration — it's the same
  source chain; a representative contradicting it is a finding.

**Employee layer (broader, automatic).** Beyond the capped leadership roster,
Company Snipe also samples ordinary employees who publicly identify as working at
the company — a broader, less bounded population than representatives, so treat
it as a *sample*, not a census. Identify via platform search (LinkedIn "current
company", X/Twitter bios, company employee-tagged content), cap the sample size,
and record the sampling basis transparently (report-format §7c). **Collection for
this layer strictly uses three fixed tracks together on every run — LinkedIn via
Anysite, X/Twitter via Xpoz, and `web_search`/`web_fetch` — always all three, not
fallback-on-failure** (`anysite-mcp-usage.md`), merged and deduplicated. The
cross-check above extends to this layer too: does what employees say publicly
match the official line, and does it match what representatives say
(report-format §7d).

**Output:** the multi-source GrowGoSnipe Report, with the source map, claim risk
map, representative roster (§7b), employee sample (§7c), and cross-source
consistency matrix (§7d) emphasized. It also ends with the **comparative
scorecard** (§7g) — the strict, fixed-pattern adverse rate (Misleading +
Contradicted, over assessed claims) plus evidence-based sector positioning — and
emits the standardized scorecard summary line so the audit is comparable to
others (`comparative-scorecard.md`).

**Constraints:** do not analyze raw scraped data immediately — collect a
manifest, deduplicate, filter by relevance / materiality / risk, then verify the
highest-risk claims. A company's self-reported figure is an interested source;
corroborate it. "All media" coverage is almost always partial in practice
(connector limits — see anysite-mcp-usage.md); state the real coverage rather than
implying completeness.

---

## 4. Person Snipe

**Input:** a public person's name, plus optional official profile links, company
bio, LinkedIn, X/Twitter, interviews, publications, conference pages, speeches,
or other public/professional sources.

**Default limit:** the **last 4 months** of public material (trailing window from
today), not a fixed item count. Keep a safety cap (≈75 items after dedup); if hit,
say so and prioritize by risk/materiality. Items with unknown dates are kept but
flagged as unconfirmed-in-window. An explicit user limit or window overrides this.

**Behavior:** collect the last 4 months of public professional / public statements
by or about the person. Focus on public claims, professional statements, role / title
claims, policy claims, market / economic claims, technical claims, quotes,
interviews, publications, and company-related claims. Produce a person-level
claim audit and an attribution-risk report.

**Constraints (privacy — strict):** public / professional information only. Do
**not** collect private addresses, private phone numbers, family details,
private accounts, sensitive attributes, leaked credentials, or any doxxing-style
data. Do not infer sensitive personal attributes. Do not focus on personal life
unless the user has provided a specific public claim that makes it directly
relevant. When in doubt, exclude. See anysite-mcp-usage.md (privacy rules).

---

## 5. Adaptive Snipe

**Input:** a free-form instruction, e.g. "last 20 articles from this site",
"last 100 posts from this company", "only claims about AI", "only statistics and
quotes", "find the weakest unsupported claims", "check only claims using first /
biggest / leading / record", "scrape the latest posts from these profiles and
rank the riskiest claims".

**Default limit:** infer a reasonable default and state it explicitly.

**Behavior:** infer the scope (mode, target, limit, date range, source types,
claim focus, exclusions, language, output depth). Confirm only if genuinely
necessary to collect reliably. **State the inferred scope and limit before
execution.** Then collect, deduplicate, filter, analyze, verify, and report
according to the requested scope, borrowing whichever mode's machinery fits.

**Output:** whichever report shape matches the inferred scope (single-item or
multi-source), tailored to the requested focus (e.g. a ranked list of the
riskiest claims when that's what was asked for).

**Constraints:** the user's explicit scope wins over the defaults. If they narrow
the claim focus ("only AI claims"), extract broadly but report on the requested
subset, noting what was set aside.

---

## Default-limit summary

| Mode | Default limit | Connector needed |
|---|---|---|
| Article Snipe | 1 item | No |
| Site / Feed Snipe | last 20 public items | Yes |
| Company Snipe | last 4 months (cap ≈150 items) + representative network | Yes |
| Person Snipe | last 4 months (cap ≈75 items) | Yes |
| Adaptive Snipe | inferred, stated | Usually |

Respect an explicit user limit over any default. If the connector returns fewer
items than the limit, report the true count — never pad to hit the number.
