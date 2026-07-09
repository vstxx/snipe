# Anysite MCP Usage

Anysite MCP is GrowGoSnipe's collection / scraping layer for the multi-source
modes. This file covers how to use it safely and effectively: discovery,
tool selection, defaults, metadata preservation, failure handling, source
limits, and privacy. The single-item Article Snipe mode does not need it — use
direct fetch and web search there.

## Connector discovery (do this first, every run)

The connector's exact tools and schemas vary and may change, so **inspect what is
actually connected before collecting** rather than assuming a fixed tool set:

1. Check the connected MCP tools for the Anysite server and read the schema of
   each candidate tool (its inputs, outputs, and limits).
2. If the Anysite connector isn't present under that name, look for another
   connected collection/scraping connector and use it, noting the substitution
   to the user.
3. If no collection connector is reachable at all, say so plainly. Fall back to
   direct fetch / web search where that can still serve the request, and scale
   back the scope honestly — never fabricate items to fill a limit.

This discovery step is deliberate: hard-coding tool names makes the skill brittle,
and the modes are defined by *what they collect*, not by a specific tool.

## Tool selection

- Use the **most specific** available tool for the target source (a
  posts-by-account tool for a social profile, a feed/section tool for a
  newsroom, a search tool for keyword-scoped collection).
- Prefer tools that return **structured output** over raw HTML — structured
  results preserve metadata and are far easier to deduplicate and audit.
- Request only the fields you need when the tool supports field selection; it's
  faster and cheaper.

## Platform tool routing

Some platforms have a dedicated connector that outperforms the general-purpose
one. Route by platform, not by habit:

| Platform | Primary tool |
|---|---|
| X / Twitter | **Xpoz** (`Xpoz:getTwitterPostsByUser`, `Xpoz:getTwitterPostsByKeywords`, `Xpoz:getTwitterUsersByKeywords`, etc.) |
| LinkedIn | **Anysite** |
| Everything else (news sites, YouTube, Reddit, TikTok, Instagram, general web) | Anysite if it covers the source; otherwise `web_fetch`/`web_search` |

If the named tool for a platform isn't connected or fails, fall back to the other
connector or to `web_fetch`/`web_search`, and note the substitution — the routing
table is a preference, not a hard requirement, **except** for employee-layer
collection, which has its own stricter rule below.

## Employee-layer collection — strict dual-source order

For the **employee layer** in Company Snipe (broad, rank-and-file employees —
distinct from the capped representative roster), the normal
primary-connector-with-fallback pattern is not enough: employees are diffuse and
easy to under-collect with a single source. For this layer specifically, hit
**three fixed tracks on every run**, always all three, never either/or and never
fallback-on-failure:

- **LinkedIn via Anysite** — always run it; LinkedIn is where employment is most
  explicitly self-declared, so it must always be used, not just when other tracks
  come up short.
- **X/Twitter via Xpoz** — always run it for employee posts on X.
- **`web_search` / `web_fetch`** — always run it as the third, mandatory pass to
  catch employees surfaced outside those two platforms.

Merge and deduplicate across all three. If one track returns nothing, say so and
keep the others' results — a partial or empty return from one track is never a
reason to skip another. This rule maximizes recall on a hard-to-enumerate
population; it applies to the employee layer only, not to official company
sources or the representative roster, which keep the normal primary/fallback
pattern.



Preserve, for every collected item, and carry into the manifest verbatim:

- URL and **canonical_url**
- **timestamp** (published_at) and collected_at
- author / account
- source_platform (e.g. X, LinkedIn, YouTube, a named news site)
- source_type (official site, newsroom, press release, social, media, docs, …)

Never fabricate a missing field. If a timestamp or author is absent, record it as
unknown — a missing date is itself an audit finding, not a blank to be filled.

## Defaults and source limits

- Article Snipe: 1 item (no connector).
- Site / Feed Snipe: last 20 public items.
- Company Snipe: last 4 months of public material (safety cap ≈150 items after
  dedup), plus the representative network (board / directors / spokespeople /
  ambassadors) over the same window with a per-person cap.
- Person Snipe: last 4 months of public material (safety cap ≈75 items after dedup).
- Adaptive Snipe: infer a reasonable limit/window and state it.

Respect an explicit user limit over the default. If the connector returns fewer
items than requested, report the true count. For large runs, build the compact
source-manifest summary first rather than streaming raw content.

## Partial access and paywalls

Some collected items expose metadata but not full text — paywalled posts
(`is_paywalled: true`, audience `only_paid`), members-only pages, video with no
transcript, or truncated feeds. Handle these honestly rather than guessing:

- Mark the item `text_available: false` in the manifest and record why (paywalled,
  no transcript, etc.).
- Extract claims **only from the metadata you actually have** — title, subtitle,
  preview. A superlative in a paywalled headline ("largest in modern history") is
  a real, extractable claim, but it can only be flagged as an unverified
  title-level claim, never rated from a body you couldn't read.
- Never infer or reconstruct body content for a paywalled item. If a headline
  claim matters, put it in Manual review with a note that the body is inaccessible.
- Report the count of text-unavailable items in Limitations, so the reader knows
  how much of the run was metadata-only.

## Feed-integrity check

When collecting "the latest N" from a feed, don't assume the connector returned a
clean, contiguous, most-recent slice. Before building on it, sanity-check:

- **Chronology:** sort by timestamp and confirm the items are actually the most
  recent and roughly contiguous. A gap (e.g. a missing run of months) or
  out-of-order items means the feed's ordering isn't purely chronological, or
  some items were filtered/omitted — flag it in Limitations rather than presenting
  the set as "the latest N."
- **Count:** if the feed returned fewer items than requested, report the true
  number; don't pad.
- **Filtering side-effects:** note when paywalled or non-article items inflate or
  distort the "recent" set.

These checks are cheap and prevent the audit from silently misrepresenting its own
sample.

## Failure handling

- **Source inaccessible / blocked:** state the limitation clearly and continue
  with what you could collect; note the gap in the report's Limitations.
- **Partial results / rate limits:** report what you got and that it's partial.
- **Ambiguous target** (name resolves to several entities): ask one clarifying
  question before collecting.
- **Empty result:** say the source returned nothing; don't infer what "would"
  have been there.

## Privacy and safety rules (strict)

These are hard limits and override any convenience:

- **Public information only**, unless the user explicitly provides authorized
  material and clearly has lawful access.
- Do **not** collect private addresses, private phone numbers, family details,
  private accounts, sensitive attributes, leaked credentials, or doxxing-style
  data.
- Do **not** assert that a text was AI-written as a bare claim, and never
  attribute AI authorship to a named real person. The one narrow exception is
  the hedged, isolated, flagged-items-only advisory signal defined in
  `ai-style-signal.md` — a confidence band on stylistic markers, never a binary
  call. Outside that exact process, authorship detection is out of scope.

If a request would require crossing any of these lines, decline that part,
explain why briefly, and offer the public-data version of the task.
