# Collection Workflow

This is the collection stage for the multi-source modes (Site/Feed, Company,
Person, and multi-source Adaptive). It runs *before* the audit phases. Its whole
purpose is to turn a messy pile of public items into a clean, deduplicated,
prioritized manifest so verification effort lands on the claims that matter.

The governing rule: **do not analyze raw scraped data immediately. First build a
source manifest, deduplicate, filter by relevance / materiality / risk, then
verify the highest-risk claims.** Skipping straight to analysis means auditing
duplicates, syndicated reposts, and marketing filler while the load-bearing
claims wait.

Follow these steps in order.

## 1. Scope

Fix the parameters before collecting: mode, target, limit, date range, source
types, claim focus, exclusions, language, and output depth. Ask a clarifying
question only if the target is too ambiguous to collect reliably (e.g. a company
name that maps to several distinct entities). Otherwise infer, and for Adaptive
mode state the inferred scope and limit before executing.

For **Company and Person modes the default scope is a trailing time window — the
last 12 months from today — not a fixed item count.** Compute the window boundary,
filter by `published_at`, and keep a safety cap on item volume (see sniping-modes.md)
so a busy target doesn't overwhelm the run; if the cap bites, prioritize by
risk/materiality and say so. Items whose date can't be established are kept but
flagged as unconfirmed-in-window rather than silently dropped or silently included.
An explicit user window or count overrides the default.

If an audit **goal** was supplied, record it in scope now. It reweights the
relevance/risk filtering below toward goal-critical claims and drives the Goal
readout — but never filters what gets collected or extracted away from the goal
(`references/goal-directed-audit.md`).

## 2. Source discovery

Find the right sources for the mode:

- **Company:** official site, newsroom, blog, press releases, product pages,
  investor relations / docs / filings if relevant, LinkedIn, X/Twitter, YouTube,
  reputable interviews and media. **Also the representative network** — identify
  the board, C-suite / directors, official spokespeople, and brand ambassadors
  from official sources, cap the roster by seniority / public prominence, and
  collect each one's public company-related statements over the same 12-month
  window (per-person cap). Apply Person-mode privacy rules to every individual
  (public/professional only). Record who was included and who was excluded and
  why — this becomes the roster in report §7b.

  **Also the employee layer** — broader than the representative roster: ordinary
  employees who publicly identify as working at the company (LinkedIn "current
  company" field, X/Twitter bios, a company "life at X" tag). Identify via
  platform search rather than trying to enumerate the whole company; cap the
  sample (e.g. top N by visibility/engagement within the window) and, exactly as
  with representatives, record who was swept in and the basis for the cap — this
  becomes the employee sample in report §7c. Same Person-mode privacy rules apply:
  public/professional statements only, never private life or sensitive
  attributes. **Collection for this layer always uses three fixed tracks together
  — LinkedIn via Anysite, X/Twitter via Xpoz, and `web_search`/`web_fetch` — all
  three every run, not fallback-on-failure** (see the strict rule in
  `anysite-mcp-usage.md`), merged and deduplicated.
- **Person:** official bio, company profile, public social profiles, interviews,
  speeches, publications, conference pages, professional pages.
- **Site / source:** homepage, section page, RSS/feed, sitemap, chronological
  article list, internal search if available.

Use the most specific collection tool available for each source type (see
anysite-mcp-usage.md). Prefer official / primary sources first.

## 3. Source manifest

Build the manifest **before** any deep analysis. One row per collected item,
using the fields in `references/source-manifest-schema.md`: item_id, title, URL,
canonical_url, source_platform, source_type, author/account, published_at,
collected_at, language, text_available, content_length, duplicate_group,
relevance_score, materiality_score, preliminary_risk_score, detected_claim_count,
notes. Preserve URLs, timestamps, authors/accounts, platform, and canonical URLs
exactly as collected — never fabricate a missing field; mark it unknown.

**Manifest depth — match the structure to the run size.** For a small run
(roughly ≤25 items), you don't need to emit the full per-item structured manifest;
tracking the fields internally and presenting the summary table (§3) plus the
item-level risk table (§4) is sufficient, as long as provenance is preserved
(every claim traces to an item, every item to a source). For larger runs (the
100-item Company Snipe, big Adaptive pulls), emit the full structured per-item
manifest so dedup, filtering, and prioritization are auditable at scale. The point
is that the discipline (manifest-first, deduplicate, filter, then verify) always
holds; only the level of printed structure scales with size.

## 4. Deduplication

Collapse duplicates by canonical URL, title similarity, timestamp,
account/author, near-duplicate text, syndicated reposts, and copied press
releases. Assign a duplicate_group to clustered items and keep the strongest /
canonical source as the representative. This directly enforces "repetition is not
corroboration": the same press release across ten outlets is one source chain,
recorded once, not ten confirmations.

## 5. Relevance and risk filtering

Prioritize items and claims carrying: numbers, customers, products, revenue,
funding, market position, AI / technical claims, legal / regulatory claims,
quotes, role / title claims, rankings / superlatives ("first / biggest /
leading / record"), causal claims, recency, and claims repeated across official
sources. Down-weight low-materiality marketing filler — don't spend verification
budget on slogans. Honor any user-specified claim focus here.

## 6. Claim extraction

Extract claims item by item (don't blend items together — you lose provenance).
Every claim record includes:

- `claim_id`
- `item_id`
- `claim_source_url` — hard link to the exact source the claim came from (the
  item's canonical URL); mark unknown only when there genuinely is no URL
- `claim_source_locator` — where inside that link the claim sits: a text-fragment
  anchor (`#:~:text=…`) for web pages, a paragraph/section reference, a video
  timestamp, or a post permalink. This is origin, not evidence — never fabricate
- `claim_text` — restated neutrally in your own words
- `original_sentence` — the source sentence, kept short (copyright discipline)
- `claim_maker` — article/editorial voice, quoted person, referenced report,
  named institution, unnamed experts, or unclear
- `type` — statistical, causal, historical, attribution/quote, predictive,
  comparative, definitional, existence (see claim-taxonomy.md)
- `entities` — companies, people, products, places, figures involved
- `date/scope` — the time period and scope the claim covers (flag if missing)
- `evidence_needed` — what would confirm or refute it
- `materiality` — High / Medium / Low
- `risk` — preliminary risk that it's wrong, unsupported, or misleading

## 7. Evidence verification

Verify the highest-risk / highest-materiality claims in detail (top 5–10 for a
typical run) unless the user asks for exhaustive analysis. Prefer primary and
authoritative sources (source-hierarchy.md). Apply the discipline from the
single-item audit: repetition is not corroboration; a press release repeated by
media is still one source chain; lack of evidence means **unsupported, not
false**; trace statistics to the data producer and quotes to the primary record.

Before finalizing any **Contradicted** verdict or document-level **Integrity
flag**, run the self-check in `references/flag-integrity-check.md` — confirm the
call is grounded in specific, actually-retrieved, re-read evidence rather than
something the model asserted without verifying. Downgrade and say so if it
doesn't hold up; the check only pulls a flag back, never raises confidence.

## 8. Aggregate analysis

Look across items for patterns a single-item audit can't see: recurring
unsupported claims, vague authority language reused across posts, the same
statistic repeated without methodology, product/market claims asserted without
evidence, internal contradictions between items, which source types were
strongest, which were weakest, and the overall integrity pattern. This aggregate
view is often the most valuable output of a multi-source run.

For **Company mode**, add the **cross-source consistency check**: line up what the
company says officially against what its representatives *and employees* say
publicly (and against each other). Flag conflicts, rep-or-employee-only claims
the company hasn't confirmed (attribution risk), and timing mismatches. Remember
a representative or employee echoing the company line is the same source chain,
not independent corroboration. Report it as the cross-source consistency matrix
(report §7d).

For **Company mode** (and any multi-source run on request), produce the
**comparative scorecard** (`references/comparative-scorecard.md`): the strict,
fixed-pattern adverse rate ((Misleading + Contradicted) / assessed claims), its
band, and — only from a real basis, never invented — the company's positioning
against its sector. Emit the standardized one-line summary record so the run
becomes comparable to others. The scorecard counts verdicts; it never changes
them.

Also across **all multi-source modes**, run a **recurring / patterned claims**
pass over the whole run's claim ledger: find claims that are identical or
near-identical in wording across different items/people, and mark them as a
pattern rather than as separate corroborating claims. A cluster of near-verbatim
phrasing across many authors is a signal of a shared script or coordinated
messaging, not independent confirmation — this directly extends "repetition is
not corroboration" from a single duplicate-detection step (§4) to a whole-run
pattern report. Report it as the patterned-claims table (report §7e).

For **flagged items** (those carrying a high-risk / flagged claim), optionally run
the AI-style pass (`ai-style-signal.md`) and report it as an isolated, hedged
advisory — never mixed into the factual verdicts and never in the scores.

## 9. Report

Produce the findings-first multi-source report (report-format.md): scope, source
map, source manifest summary, item-level risk table, top claim risks, detailed
verification of the top claims, aggregate pattern analysis, manual-review
recommendations, scoring (four axes + composite), and a final judgement.

## Honesty constraints throughout

- If the connector can't access a source, state the limitation; don't guess its
  contents.
- If fewer items are available than requested, report the real number.
- Never invent posts, dates, metadata, authors, or sources.
- Don't dump raw scraped content into the final answer — summarize via the
  manifest and surface only what the findings require.
