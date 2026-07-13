# Source Manifest Schema

The manifest is the backbone of every multi-source run. It is built during
collection, *before* deep analysis, and it is what deduplication, filtering, and
prioritization operate on. Keep it structured and honest: every field either
holds real collected data or is explicitly marked unknown — never fabricated.

**How much structure to print scales with run size.** The schema below is the
full form. For small runs (≈≤25 items) you may keep these fields as internal
working state and present only the report's summary table (§3) and item-level
risk table (§4) — the discipline still runs, just not the full printout. For
large runs (100-item Company Snipe, big Adaptive pulls) emit the full per-item
structured manifest so the collection stage is auditable at scale. Provenance is
non-negotiable at any size: every claim traces to an item, every item to a source.

## Collected item

One record per collected item.

```json
{
  "item_id": "it_001",
  "title": "string — item headline/title, or unknown",
  "url": "https://… — as collected",
  "canonical_url": "https://… — canonical form if available, else same as url",
  "source_platform": "e.g. X, LinkedIn, YouTube, company-site, news-site",
  "source_type": "official-site | newsroom | blog | press-release | social | media-interview | report-doc-filing | third-party-context",
  "author": "author or account handle, or unknown",
  "published_at": "ISO 8601 datetime, or unknown",
  "collected_at": "ISO 8601 datetime of collection",
  "language": "ISO code, e.g. en, pl",
  "text_available": true,
  "content_length": 0,
  "duplicate_group": "dg_00 — shared id for near-duplicates; unique if no dup",
  "relevance_score": 0,
  "materiality_score": 0,
  "preliminary_risk_score": 0,
  "detected_claim_count": 0,
  "notes": "collection caveats, access issues, why prioritized/deprioritized"
}
```

Field notes:

- **canonical_url** drives deduplication; capture it wherever the source exposes
  one. Two items sharing a canonical_url are the same item.
- **source_type** feeds the source map and the strong/weak-source analysis. Be
  precise: an official press release and a third-party blog about the same news
  are different types with different weight.
- **duplicate_group** clusters syndicated reposts and copied press releases;
  members of a group collapse to one representative for verification (repetition
  is not corroboration).
- **relevance / materiality / preliminary_risk scores** are 0–100 working
  numbers used only to prioritize; they are not the final audit scores. Higher
  materiality + higher risk = verify first.
- **text_available / content_length** flag items where the body couldn't be
  collected (e.g. video with no transcript) so they aren't mistaken for empty.

## Claim record

Emitted during per-item claim extraction (step 6 of the collection workflow),
linked back to its item.

```json
{
  "claim_id": "cl_001",
  "item_id": "it_001",
  "claim_source_url": "https://… — hard link to the exact source the claim was extracted from; the item's canonical_url, or unknown if pasted text with no URL",
  "claim_source_locator": "WHERE inside that link the claim appears: a text-fragment anchor (#:~:text=…) for web pages, a paragraph/section reference, a video timestamp, or a post permalink. Never fabricate — mark 'pasted text, no URL' when there is none",
  "claim_text": "neutral restatement in your own words",
  "original_sentence": "short source snippet (keep brief — copyright)",
  "claim_maker": "article/editorial voice | quoted person | referenced report | named institution | unnamed experts | unclear",
  "type": "statistical | causal | historical | attribution/quote | predictive | comparative | definitional | existence",
  "entities": ["company", "person", "product", "figure", "place"],
  "date_scope": "period/scope the claim covers, or 'missing — flag'",
  "evidence_needed": "what would confirm or refute it",
  "materiality": "High | Medium | Low",
  "risk": "High | Medium | Low — preliminary likelihood it's wrong/unsupported/misleading",
  "verdict": "filled at verification: Supported | Mostly supported | Mixed | Misleading | Unsupported | Contradicted | Outdated | Unverifiable",
  "confidence": "High | Medium | Low — set at verification",
  "evidence": "sources relied on; filled at verification"
}
```

## Manifest summary (for the report)

Don't print the full manifest in the answer. Aggregate it into the report's
source-manifest-summary table (by source_type): item count, how many had text,
total claim count, and preliminary risk. The full manifest is working state; the
summary is what the reader sees.

| Source type | Items | Text available | Claim count | Preliminary risk |
|---|---:|---:|---:|---:|

## Integrity

- Real data or explicit `unknown` — no invented titles, dates, authors, or URLs.
- Preserve collected values exactly; don't normalize away information.
- Keep provenance: every claim traces to an item, every item to a source.
- **Origin vs. evidence are two different things.** `claim_source_url` +
  `claim_source_locator` record where the claim was *made* (the audited
  text/post); `evidence` records what it was *checked against* (external
  sources). Never conflate them, and never put a verification source in the
  origin fields. For single-item runs the origin URL is the same for every claim
  (the one audited document); the locator is what varies and must still be filled.
