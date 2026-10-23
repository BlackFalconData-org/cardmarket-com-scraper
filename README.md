# Cardmarket Scraper [Just 💰$0.14]

Extract structured data from [cardmarket.com](https://cardmarket.com) — Get the Cardmarket price guide for 23 trading card games: trend, low, average and 1/7/30-day prices, plus foil and holo variants, from Cardmarket's own public catalog. Structured rows, ready for a spreadsheet or price tracker.

**[Cardmarket Scraper [Just 💰$0.14] on Apify →](https://apify.com/blackfalcondata/cardmarket-com-scraper?fpr=1h3gvi)**

---

## 🚀 How to use this actor

> ### 💚 $5 free Apify credits — every month
> No credit card required. No commitment. Cancel anytime.

### 👉 [Sign up free on Apify →](https://console.apify.com/sign-up?fpr=1h3gvi)

1. **Click sign up** — pick GitHub, Google, or email; takes ~30 seconds
2. **Open this actor** — input is pre-filled with a working example
3. **Click Start** — export results as JSON, CSV, or Excel

Your **$5 monthly platform credit** is enough to run this actor right away — and again every month — scraping typically several hundred to several thousand results per run, depending on your input.

## Key features

**Search with filters** — Search by keyword and location. Filter by 📝 description format, and more.

**Incremental mode** — Only get new or changed products since your last run. Content hash per product — no duplicates, no re-processing.

**Change classification** — Track unchanged, expired, cross-run duplicate detection across runs. Build audit trails of how products evolve over time.

**Compact output** — Emit core fields only (AI-agent / MCP-friendly). Keeps response size small for LLM workflows.

**Result cap** — Stop after N products (up to 100.000). Set to 0 for the full catalog.

**Export anywhere** — Download as JSON, CSV, or Excel. Stream via Apify API, webhooks, or integrations with Make, Zapier, Airbyte, Keboola.

**Structured data** — Every product returns the same schema with consistent field naming. All fields always present — `null` when unavailable, never omitted.

---

## Use cases

**Data pipeline automation**
Integrate with your ETL pipeline to collect structured products from cardmarket.com on a schedule. Export to CSV, JSON, or directly to your database. Use compact mode to control output size.

**Market research**
Monitor products, track trends, and analyze market dynamics with structured, deduplicated data from cardmarket.com.

**Change monitoring**
Run daily or hourly in incremental mode to capture only new, updated, or expired ${NOUNS}. Perfect for price-tracking, churn analysis, and alerting pipelines.

**AI / LLM training data**
Structured JSON per product is ready for RAG pipelines, embeddings, and agent workflows. Compact mode trims tokens for LLM context windows.

---

## Quick start

```json
{
  "query": "Black Lotus",
  "location": "magic",
  "maxResults": 100,
  "compact": false,
  "excludeEmptyFields": false,
  "incrementalMode": false,
  "skipReposts": false,
  "notifyOnlyChanges": false,
  "notificationLimit": 5,
  "descriptionFormat": "all"
}
```

---

## Input parameters

| Parameter | Type | Default | Description |
|-----------|------|---------|-------------|
| `query` | string | — | Matches any product whose name contains this text (case-insensitive), e.g. "Black Lotus" or "Booster Box". Leave empty to return the whole catalog for the selected game. Use a JSON array (e.g. ["Black Lotus","Mox Ruby"]) to run several searches in one run. |
| `location` | enum | `"magic"` | Which trading card game's catalog and price guide to read. Use a JSON array (e.g. ["magic","pokemon"]) to cover several games in one run. |
| `maxResults` | integer | `100` | Maximum total products returned (0 = unlimited). |
| `startUrls` | array | `[]` | Paste raw Cardmarket product-search URLs. Each URL becomes its own search task; results are merged and deduped by product across all URLs. When provided, startUrls REPLACE the search term / game fields (unsupported URLs are skipped, and if none is supported the run stops with an input error). |
| `compact` | boolean | `false` | Core fields only (for AI-agent/MCP workflows). |
| `excludeEmptyFields` | boolean | `false` | Drop null, empty-string, and empty-array fields from each record before push. Smaller payloads for AI agents and dashboards. |
| `incrementalMode` | boolean | `false` | Compare against previous run state and report only products whose price guide moved. stateKey is optional — defaults to a value derived from your search inputs (search terms, games, startUrls) so different filter sets never share state. |
| `stateKey` | string | — | Optional. Stable identifier for the tracked product universe. Leave empty to auto-generate from search inputs. |
| `emitUnchanged` | boolean | `false` | When Incremental Mode is on, also return products whose price guide did not move since the previous run (marked UNCHANGED). Off by default: only new and changed products are returned. |
| `emitExpired` | boolean | `false` | When Incremental Mode is on, also return products that disappeared from the game's catalog since the previous run (marked EXPIRED). Scans the whole catalog of the selected game, so Max Results does not limit this run. |
| `skipReposts` | boolean | `false` | When incremental, skip products whose content matches an expired product from a prior run (cross-run duplicate detection). |
| `notifyOnlyChanges` | boolean | `false` | When Incremental Mode is on, only send notifications for NEW and UPDATED products. Has no effect outside incremental mode. |
| `notificationLimit` | integer | `5` | Maximum number of products included in each notification message (1–20). |
| `telegramToken` | string | — | Telegram bot token (from @BotFather). Required for Telegram notifications. |
| `telegramChatId` | string | — | Telegram chat or channel ID (e.g. "-100123456789"). Required when telegramToken is set. |
| `discordWebhookUrl` | string | — | Discord incoming webhook URL. Server Settings → Integrations → Webhooks → New Webhook. |
| `slackWebhookUrl` | string | — | Slack incoming webhook URL. api.slack.com/messaging/webhooks. |
| `whatsappPhoneNumberId` | string | — | Your WhatsApp Business phone-number ID (numeric, from Meta dashboard). Required when whatsappAccessToken is set. |
| `whatsappAccessToken` | string | — | WhatsApp Cloud API permanent access token (System User token from Meta Business). Recipient must have messaged the business number within the last 24h (service-conversation window — free since Nov 2024). |
| `whatsappTo` | string | — | Recipient phone in E.164 format without + (e.g. "436641234567"). Recipient must have messaged your business number within last 24h. |
| `webhookUrl` | string | — | Receives a JSON POST with {metadata, items} after each run. Universal escape hatch for n8n / Make / Zapier / custom backends. |
| `webhookHeaders` | object | — | Optional JSON object of custom headers (e.g. {"Authorization":"Bearer ..."}). |
| `appConnector` | string | — | Optional. Pick a connected app under Settings → API & Integrations to receive your results. Best-effort across MCP connectors as Apify expands its catalog. |
| `mcpIssueTeam` | string | — | Only when the connected app is an issue tracker: the team (name or ID) the summary issue is created under, if that app requires one. |
| `descriptionFormat` | enum | `"all"` | Choose which representation of the listing description to include. `all` keeps every variant; the others keep only the selected one. |

---

## Output fields

Every product returns the same 63-field schema. Missing values are `null` — never omitted.

- `idProduct`
- `name`
- `game`
- `gameSlug`
- `productType`
- `categoryId`
- `categoryName`
- `idExpansion`
- `expansionName`
- `expansionSource`
- `dateAdded`
- `currency`
- `priceGuideDate`
- `trendEur`
- `lowEur`
- `avgEur`
- `avg1DayEur`
- `avg7DayEur`
- `avg30DayEur`
- `variantType`
- `variantTrendEur`
- `variantLowEur`
- `variantAvgEur`
- `variantAvg1DayEur`
- `variantAvg7DayEur`
- `variantAvg30DayEur`
- `momentum7Pct`
- `momentum30Pct`
- `trendVsLowPct`
- `variantPremiumPct`
- `searchUrl`
- `source`
- `scrapedAt`
- `contentHash`
- `changeStatus`
- `idMetacard`
- `productUrl`
- `portalUrl`
- `fromPrice`
- `fromPriceRaw`
- `priceTrend`
- `priceTrendRaw`
- `avg30d`
- `avg30dRaw`
- `avg7d`
- `avg7dRaw`
- `avg1d`
- `avg1dRaw`
- `fromPriceFoil`
- `fromPriceFoilRaw`
- `priceTrendFoil`
- `priceTrendFoilRaw`
- `avg30dFoil`
- `avg30dFoilRaw`
- `avg7dFoil`
- `avg7dFoilRaw`
- `avg1dFoil`
- `avg1dFoilRaw`
- `listingId`
- `searchQuery`
- `contentQuality`
- `detailFetched`
- `changeType`

---

## Sample output

One object per product. Here is a real example from a production run:

```json
{
  "idProduct": 273699,
  "name": "Charizard [Energy Burn | Fire Spin]",
  "game": "Pokémon",
  "gameSlug": "pokemon",
  "productType": "single",
  "categoryId": 51,
  "categoryName": "Pokémon Single",
  "idExpansion": 1523,
  "expansionName": null,
  "expansionSource": null,
  "dateAdded": null,
  "currency": "EUR"
}
```

*Truncated — full records contain 63 fields. See Output fields for the complete schema.*

**[Try Cardmarket Scraper [Just 💰$0.14] now — $5 free credit, no credit card →](https://apify.com/blackfalcondata/cardmarket-com-scraper?fpr=1h3gvi)**

---

## Pricing

Pay only for what you extract. No subscription required — Apify's free $5 credit covers thousands of results.

| Event | Price (USD) |
| --- | --- |
| Actor Start | $0.005 |
| Result | $0.0005 |

See the [actor on Apify](https://apify.com/blackfalcondata/cardmarket-com-scraper?fpr=1h3gvi) for current pricing.

---

## FAQ

**How do I scrape cardmarket.com?**
Use this actor on Apify to extract structured data from cardmarket.com. Configure your search query and filters in the input, then click Start — no coding required.

**How do I get cardmarket.com data as JSON, CSV, or Excel?**
The actor writes each product to Apify's dataset. Download as JSON, CSV, or Excel from the Console, stream via the API, or push to Make, Zapier, Airbyte, or Keboola.

**Is it legal to scrape cardmarket.com?**
Web scraping of publicly available data is generally legal. This actor only accesses publicly visible information. Always check cardmarket.com's terms of service for your specific use case.

**How much does it cost?**
Pay-per-event pricing — you only pay for ${NOUNS} extracted. Apify's free $5 credit is enough to run thousands of results before you pay anything.

**How does incremental mode work?**
Each product gets a content hash. On subsequent runs, only new or changed products are emitted — saving time, compute, and storage. Expired products can be tracked separately.

**Do I need an API key or credentials?**
No. Just sign up for Apify, paste your input, and click Start. No credit card required.

---

## Related products by Black Falcon Data

[Browse all Black Falcon Data actors →](https://apify.com/blackfalcondata?fpr=1h3gvi)

---

## Getting started with Apify

New to Apify? [Create a free account with $5 credit](https://console.apify.com/sign-up?fpr=1h3gvi) — no credit card required.

1. Sign up — $5 platform credit included
2. Open [Cardmarket Scraper [Just 💰$0.14]](https://apify.com/blackfalcondata/cardmarket-com-scraper?fpr=1h3gvi) and configure your input
3. Click **Start** — export results as JSON, CSV, or Excel

Need more later? [See Apify pricing](https://apify.com/pricing?fpr=1h3gvi).

---

## About Black Falcon Data

Black Falcon Data builds production-grade web scrapers for job boards and marketplace data. Browse our full actor catalog at [www.blackfalcondata.com](https://www.blackfalcondata.com).

---

*Last updated: 2026 10*
