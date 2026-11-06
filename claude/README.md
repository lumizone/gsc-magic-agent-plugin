# GSC Magic for Claude

Your own Google Search Console, Bing Webmaster Tools, Google Analytics 4, IndexNow and Core Web Vitals
data in Claude (claude.ai, Cowork and Claude Code), through the hosted [GSC Magic](https://gscmagic.com)
MCP server.

Ask things like:

- "Which queries on example.com are stuck in positions 4 to 20 with lots of impressions?"
- "Why did organic clicks drop last week? Show me the pages that lost the most."
- "Which pages compete for the same keyword?"
- "Are the URLs in my sitemap actually indexed?"
- "How much traffic do ChatGPT and Perplexity send me according to GA4?"

## What is in this plugin

- **The GSC Magic connector**: the remote MCP server `https://mcp.gscmagic.com/mcp` (`.mcp.json`).
  It is the same server as the GSC Magic connector in the Claude directory, so people who have both see
  one set of tools. It brings 69 tools: Search Console analytics, URL inspection and sitemaps, Bing
  Webmaster Tools, GA4 reports, CrUX and PageSpeed, IndexNow, and finished SEO reports (quick wins,
  traffic drops, content decay, cannibalization, CTR outliers, brand split, index health, Google vs Bing)
  with optional PNG charts. It also offers six MCP prompts, such as `weekly_seo_report`.
- **The `gsc-magic` skill**: tells Claude how to use those tools well: discover the sites first, prefer
  the finished reports over raw rows, respect the 2 to 3 day Search Console lag, and never run a write
  tool without the user's go-ahead.

## Set up

1. Add the plugin from the Claude directory, or in Claude Code:

   ```bash
   claude plugin install gsc-magic --marketplace lumizone/gsc-magic-agent-plugin
   ```

2. Connect the connector: on claude.ai and in Cowork from the plugin's **Connectors** tab, in Claude Code
   with `/mcp`. Sign in to GSC Magic with Google or e-mail and allow access. A free account is available
   at https://app.gscmagic.com/login?mode=signup; new accounts pick which Search Console properties to
   import, and only imported properties are visible to Claude.
3. Bing data needs your Bing Webmaster API key and GA4 data needs a property linked to the site, both
   set in the dashboard under Sources (https://app.gscmagic.com/sources).

## Plans

Free: 1 site, 200 tool calls a month (10 a day), 14 days of stored history. Pro ($19 a month) and Agency
($34 a month): no call limit, more sites, 16 months of history, write tools, alerts. Details:
https://gscmagic.com/pricing

## What this plugin sends and runs

- Every tool call goes to GSC Magic's hosted server (`mcp.gscmagic.com`), which calls Google Search
  Console, Bing Webmaster Tools, GA4, IndexNow, CrUX and PageSpeed Insights with the user's own connected
  accounts. GSC Magic logs each call (tool name, site, client name, status, duration, time) for 30 days
  so the user can see it in the dashboard; other arguments and results are not stored in that log.
- The tools are served by that server at runtime; this plugin holds only the connector address and the
  skill. It runs nothing on your computer, has no hooks and no scripts, and sends no telemetry.
- Read-only by default. Write tools (sitemaps, site management, Bing URL submission and site settings,
  IndexNow) work only on paid plans after the user turns on write mode in the dashboard, and every tool
  carries `readOnlyHint` / `destructiveHint` annotations.
- Tool calls count toward the user's GSC Magic plan.

Privacy policy: https://app.gscmagic.com/privacy · Terms: https://app.gscmagic.com/terms ·
Contact: contact@gscmagic.com

## License

MIT. GSC Magic itself is a hosted service.
