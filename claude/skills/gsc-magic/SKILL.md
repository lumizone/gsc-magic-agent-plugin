---
name: gsc-magic
description: Answer SEO and search-traffic questions from the user's own Google Search Console, Bing Webmaster Tools, Google Analytics 4, IndexNow and Core Web Vitals data through the GSC Magic MCP server. Use when the user asks about clicks, impressions, rankings, queries, pages, traffic drops, quick wins, content decay, keyword cannibalization, CTR, indexing, sitemaps, Core Web Vitals, AI referrals in GA4, or Google vs Bing.
license: MIT
compatibility: Needs the GSC Magic connector (remote MCP server https://mcp.gscmagic.com/mcp, bundled with this plugin) and a GSC Magic account at https://app.gscmagic.com.
metadata:
  homepage: https://gscmagic.com
  docs: https://gscmagic.com/docs
---

# GSC Magic

GSC Magic is a hosted MCP server. Its tools read the user's own search data; nothing is scraped or estimated.
Claude may show the tools with a server prefix (for example `mcp__gsc-magic__gsc_list_sites`); this skill
uses the bare names.

## Before the first call

- The connector signs in through GSC Magic's own page (Google or e-mail): on claude.ai and in Cowork from
  the plugin's Connectors tab, in Claude Code with `/mcp`. A user without an
  account can create one at https://app.gscmagic.com/login?mode=signup. After signing in they pick
  which Search Console properties to import.
- Only properties imported in the GSC Magic dashboard (and allowed by the plan) are visible. If a site
  is missing, tell the user to add it at https://app.gscmagic.com/sites rather than guessing a URL.
- Bing needs the user's Bing API key and GA4 needs a property linked to the site, both in
  https://app.gscmagic.com/sources. A "not connected" error means that step is missing.

## How to work

1. Discover first: `gsc_list_sites`, then `ga_list_properties` or `bing_list_sites` if the question needs them.
2. Prefer the finished reports over raw rows when the question is analytical:
   - `report_quick_wins`: queries in positions 4 to 20 with many impressions
   - `report_traffic_drops`: biggest page/query losers and winners between two periods
   - `report_content_decay`: pages losing traffic over months
   - `report_cannibalization`: several pages competing for one query
   - `report_ctr_outliers`: pages with a CTR far below their position's norm
   - `report_brand_split`: brand vs non-brand traffic
   - `report_index_health`: sitemap URLs checked against the index
   - `report_google_vs_bing`: the same pages in both engines
3. Use `gsc_query_analytics` for custom cuts (dimensions date, query, page, country, device, filters).
   Use `gsc_inspect_url` for one URL's index status, `cwv_get_origin` / `cwv_history` / `psi_run_audit`
   for speed, `ga_run_report` for GA4 metrics.
4. Pass `chart: true` on `gsc_query_analytics` (with the `date` dimension), `report_traffic_drops`,
   `report_quick_wins`, `cwv_history`, `ga_run_report` or `bing_rank_traffic_stats` to get a PNG chart.
5. Search Console data lags 2 to 3 days. Compare equal periods (last 28 days vs the 28 before), treat
   small numbers as noise, and say when a result was truncated or hit a quota.

## Writes

Everything is read-only unless the user turned on write mode in GSC Magic (paid plans only). Write
tools include `gsc_submit_sitemap`, `gsc_delete_sitemap`, `gsc_add_site`, `gsc_delete_site`,
`bing_submit_urls`, `bing_submit_sitemap`, `bing_fetch_url`, the Bing site settings tools and
`indexnow_submit`. Every tool carries `readOnlyHint` / `destructiveHint` annotations. Never call a
write tool without the user's explicit go-ahead for that exact action.

## Ready-made workflows

The server also offers MCP prompts: `weekly_seo_report`, `find_quick_wins`, `diagnose_traffic_drop`,
`indexing_audit`, `content_refresh_plan`, `core_web_vitals_check`.

## Limits

Free: 1 site, 200 tool calls a month and 10 a day, 14 days of stored history. Pro and Agency: no call
limit. A limit error names the plan; point the user to https://app.gscmagic.com/billing instead of retrying.
