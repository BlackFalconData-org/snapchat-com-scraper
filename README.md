# Snapchat Profile Scraper [Just 💰$0.03]

Extract structured data from [snapchat.com](https://snapchat.com) — Snapchat public profiles of creators and brands by keyword or profile URL. Each row has subscriber count, bio, category and website plus recent stories and Spotlight videos with view counts. No login needed and ready for a spreadsheet or CRM.

**[Snapchat Profile Scraper [Just 💰$0.03] on Apify →](https://apify.com/blackfalcondata/snapchat-com-scraper?fpr=1h3gvi)**

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

**Search with filters** — Search by keyword. Filter by description format, and more.

**Detail enrichment** — Fetch full descriptions, structured metadata for each profile.

**Incremental mode** — Only get new or changed profiles since your last run. Content hash per profile — no duplicates, no re-processing.

**Change classification** — Track unchanged, expired, cross-run duplicate detection across runs. Build audit trails of how profiles evolve over time.

**Compact output** — Emit core fields only (AI-agent / MCP-friendly). Keeps response size small for LLM workflows.

**Description truncation** — Cap description length per ${NOUN} to control output size and cost.

**Result cap** — Stop after N profiles (up to 1.000). Set to 0 for the full catalog.

**Export anywhere** — Download as JSON, CSV, or Excel. Stream via Apify API, webhooks, or integrations with Make, Zapier, Airbyte, Keboola.

**Structured data** — Every profile returns the same schema with consistent field naming. All fields always present — `null` when unavailable, never omitted.

---

## Use cases

**Data pipeline automation**
Integrate with your ETL pipeline to collect structured profiles from snapchat.com on a schedule. Export to CSV, JSON, or directly to your database. Use compact mode to control output size.

**Market research**
Monitor profiles, track trends, and analyze market dynamics with structured, deduplicated data from snapchat.com.

**Change monitoring**
Run daily or hourly in incremental mode to capture only new, updated, or expired ${NOUNS}. Perfect for price-tracking, churn analysis, and alerting pipelines.

**AI / LLM training data**
Structured JSON per profile is ready for RAG pipelines, embeddings, and agent workflows. Compact mode trims tokens for LLM context windows.

---

## Quick start

```json
{
  "query": "food",
  "maxResults": 100,
  "includeDetails": true,
  "descriptionMaxLength": 0,
  "compact": false,
  "descriptionFormat": "all",
  "excludeEmptyFields": false,
  "incrementalMode": false,
  "skipReposts": false,
  "emitUnchanged": false,
  "emitExpired": false,
  "notifyOnlyChanges": false,
  "notificationLimit": 5
}
```

---

## Input parameters

| Parameter | Type | Default | Description |
|-----------|------|---------|-------------|
| `query` | string | — | Keyword, brand, or name to search Snapchat for. Required if no startUrls provided. Accepts one term, or a JSON array of terms to run several searches in one go. |
| `maxResults` | integer | `100` | Maximum total results (0 = unlimited). |
| `startUrls` | array | `[]` | Paste Snapchat URLs: profile (snapchat.com/@name), explore search (snapchat.com/explore/keyword), hashtag page (snapchat.com/tag/name), Spotlight video (snapchat.com/spotlight/id), Lens (snapchat.com/lens/id), a share link (t.snapchat.com/...) or a bare username/hashtag (ikea, @ikea, #football). Hashtag, Spotlight and share links resolve to the creator profile(s) behind them; a Lens URL (or a share link to one) returns a Lens row (recordType "lens": name, id, icon, previews, unlock link, official flag), plus the creator's profile when the lens has a public creator. Each URL becomes its own search task; results are merged and deduped by profile across all URLs. When provided, startUrls REPLACE the query field (each supported URL becomes one task; unsupported URLs are skipped, and if none is supported the run stops with an input error). |
| `usernames` | array | `[]` | Snapchat handles to scrape directly (with or without @), e.g. ikea. Each becomes a profile task, merged and deduped with Start URLs. |
| `hashtags` | array | `[]` | Snapchat hashtags (with or without #), e.g. football. The creators of the Spotlight videos on each hashtag page (about 30) are scraped as profiles. |
| `includeDetails` | boolean | `true` | Visit each profile to add bio, website, address, badge, snapcode and the story / spotlight / highlight / lens content. Turn off for a faster, search-only result set. |
| `includeSpotlightComments` | boolean | `false` | Add the public viewer comments shown under each profile's Spotlight videos: comment text, reactions, author display name and Bitmoji ids, time, reply count and Snapchat's ranking score. Snapchat shows up to about 25 top comments per video on the profile page. Reads the profile page, so it also works with Include Full Details off. Default: off. |
| `maxCommentsPerSpotlight` | integer | `0` | Keep only the top N comments per Spotlight video by Snapchat's ranking score. 0 = every comment the profile page shows. Ignored when Include Spotlight Comments is off. |
| `descriptionMaxLength` | integer | `0` | Truncate the profile bio to N characters. 0 = no truncation. |
| `compact` | boolean | `false` | Core fields only (for AI-agent/MCP workflows). |
| `descriptionFormat` | enum | `"all"` | Pick a single description representation. `all` keeps every variant; `text` / `html` / `markdown` drop the others. |
| `excludeEmptyFields` | boolean | `false` | Drop null, empty-string, and empty-array fields from each record before push. Smaller payloads for AI agents and dashboards. |
| `incrementalMode` | boolean | `false` | Compare against previous run state. stateKey is optional — defaults to a value derived from search inputs (queries, startUrls) so different filter sets never share state. |
| `stateKey` | string | — | Optional. Stable identifier for the tracked search universe. Leave empty to auto-generate from search inputs. |
| `skipReposts` | boolean | `false` | When incremental, skip profiles whose content matches a profile that disappeared in a prior run (cross-run duplicate detection). |
| `emitUnchanged` | boolean | `false` | When incremental mode is on, also emit profiles whose content has not changed since the last run. |
| `emitExpired` | boolean | `false` | When incremental mode is on, also emit profiles that were seen before but are no longer found. |
| `notifyOnlyChanges` | boolean | `false` | When Incremental Mode is on, only send notifications for NEW and UPDATED profiles. Has no effect outside incremental mode. |
| `notificationLimit` | integer | `5` | Maximum number of profiles included in each notification message (1–20). |
| `telegramToken` | string | — | Telegram bot token (from @BotFather). Required for Telegram notifications. |
| `telegramChatId` | string | — | Telegram chat or channel ID (e.g. "-100123456789"). Required when telegramToken is set. |
| `discordWebhookUrl` | string | — | Discord incoming webhook URL. Server Settings → Integrations → Webhooks → New Webhook. |
| `slackWebhookUrl` | string | — | Slack incoming webhook URL. api.slack.com/messaging/webhooks. |
| `whatsappPhoneNumberId` | string | — | Your WhatsApp Business phone-number ID (numeric, from Meta dashboard). Required when whatsappAccessToken is set. |
| `whatsappAccessToken` | string | — | WhatsApp Cloud API permanent access token (System User token from Meta Business). Recipient must have messaged the business number within the last 24h (service-conversation window — free since Nov 2024). |
| `whatsappTo` | string | — | Recipient phone in E.164 format without + (e.g. "436641234567"). Recipient must have messaged your business number within last 24h. |
| `webhookUrl` | string | — | Receives a JSON POST with {metadata, items} after each run. Universal escape hatch for n8n / Make / Zapier / custom backends. |
| `webhookHeaders` | object | — | Optional JSON object of custom headers (e.g. {"Authorization":"Bearer ..."}). |
| `appConnector` | string | — | Optional. Pick a connected app under Settings → API & Integrations to receive your results (including any contact details). Best-effort across MCP connectors as Apify expands its catalog. |
| `mcpIssueTeam` | string | — | Only when the connected app is an issue tracker: the team (name or ID) the summary issue is created under, if that app requires one. |

---

## Output fields

Every profile returns the same 37-field schema. Missing values are `null` — never omitted.

- `username`
- `name`
- `url`
- `portalUrl`
- `subscriberCount`
- `bio`
- `category`
- `subcategory`
- `websiteUrl`
- `address`
- `badge`
- `profilePictureUrl`
- `heroImageUrl`
- `snapcodeUrl`
- `profileId`
- `profileCreatedAt`
- `profileUpdatedAt`
- `storySnapCount`
- `spotlightCount`
- `highlightCount`
- `lensCount`
- `storySnaps`
- `spotlightVideos`
- `highlights`
- `lenses`
- `relatedAccounts`
- `spotlightComments`
- `input`
- `foundVia`
- `listingId`
- `searchQuery`
- `contentQuality`
- `detailFetched`
- `scrapedAt`
- `source`
- `contentHash`
- `changeType`

---

## Sample output

One object per profile. Here is a real example from a production run:

```json
{
  "username": "ikea_twimalik",
  "name": "IKEA Titanic",
  "url": "https://www.snapchat.com/@ikea_twimalik",
  "portalUrl": "https://www.snapchat.com/@ikea_twimalik",
  "subscriberCount": 0,
  "bio": "ناونیشان : سلێمانی شەست مەتری نزیک ھۆتێل تایتانیک \nکاتەکانمان ١٠:٠٠ بەیانی بۆ ١١:٠٠ شەو\nژم : 07710286222",
  "category": null,
  "subcategory": null,
  "websiteUrl": null,
  "address": null,
  "badge": 0,
  "profilePictureUrl": "https://cf-st.sc-cdn.net/aps/bolt/aHR0cHM6Ly9jZi1zdC5zYy1jZG4ubmV0L2QvY2ZDNkViWHpJUXZYVlBYZTlQU0JZP2JvPUVnMGFBQm9BTWdFRVNBSlFHV0FCJnVjPTI1._RS0,90_FMjpeg"
}
```

*Truncated — full records contain 37 fields. See Output fields for the complete schema.*

**[Try Snapchat Profile Scraper [Just 💰$0.03] now — $5 free credit, no credit card →](https://apify.com/blackfalcondata/snapchat-com-scraper?fpr=1h3gvi)**

---

## Pricing

Pay only for what you extract. No subscription required — Apify's free $5 credit covers thousands of results.

| Event | Price (USD) |
| --- | --- |
| Actor Start | $0.01 |
| Result | $0.000029 |

See the [actor on Apify](https://apify.com/blackfalcondata/snapchat-com-scraper?fpr=1h3gvi) for current pricing.

---

## FAQ

**How do I scrape snapchat.com?**
Use this actor on Apify to extract structured data from snapchat.com. Configure your search query and filters in the input, then click Start — no coding required.

**How do I get snapchat.com data as JSON, CSV, or Excel?**
The actor writes each profile to Apify's dataset. Download as JSON, CSV, or Excel from the Console, stream via the API, or push to Make, Zapier, Airbyte, or Keboola.

**Is it legal to scrape snapchat.com?**
Web scraping of publicly available data is generally legal. This actor only accesses publicly visible information. Always check snapchat.com's terms of service for your specific use case.

**How much does it cost?**
Pay-per-event pricing — you only pay for ${NOUNS} extracted. Apify's free $5 credit is enough to run thousands of results before you pay anything.

**How does incremental mode work?**
Each profile gets a content hash. On subsequent runs, only new or changed profiles are emitted — saving time, compute, and storage. Expired profiles can be tracked separately.

**Do I need an API key or credentials?**
No. Just sign up for Apify, paste your input, and click Start. No credit card required.

---

## Related products by Black Falcon Data

[Browse all Black Falcon Data actors →](https://apify.com/blackfalcondata?fpr=1h3gvi)

---

## Getting started with Apify

New to Apify? [Create a free account with $5 credit](https://console.apify.com/sign-up?fpr=1h3gvi) — no credit card required.

1. Sign up — $5 platform credit included
2. Open [Snapchat Profile Scraper [Just 💰$0.03]](https://apify.com/blackfalcondata/snapchat-com-scraper?fpr=1h3gvi) and configure your input
3. Click **Start** — export results as JSON, CSV, or Excel

Need more later? [See Apify pricing](https://apify.com/pricing?fpr=1h3gvi).

---

## About Black Falcon Data

Black Falcon Data builds production-grade web scrapers for job boards and marketplace data. Browse our full actor catalog at [www.blackfalcondata.com](https://www.blackfalcondata.com).

---

*Last updated: 2026 10*
