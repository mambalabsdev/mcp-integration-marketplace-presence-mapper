# Integration and Marketplace Presence Mapper MCP Server

[![LightNow capabilities](https://lightnow.ai/badge/com.mambabuilt/mcp-integration-marketplace-presence-mapper)](https://lightnow.ai/servers/com.mambabuilt/mcp-integration-marketplace-presence-mapper) [![Glama score](https://glama.ai/mcp/servers/mambalabsdev/mcp-integration-marketplace-presence-mapper/badges/score.svg)](https://glama.ai/mcp/servers/mambalabsdev/mcp-integration-marketplace-presence-mapper) [![MCP Registry](https://img.shields.io/badge/dynamic/json?url=https%3A%2F%2Fregistry.modelcontextprotocol.io%2Fv0%2Fservers%3Fsearch%3Dcom.mambabuilt%252Fmcp-integration-marketplace-presence-mapper%26limit%3D1&query=%24.servers%5B0%5D._meta%5B%22io.modelcontextprotocol.registry%2Fofficial%22%5D.status&label=mcp%20registry&color=blue)](https://registry.modelcontextprotocol.io/v0/servers?search=com.mambabuilt/mcp-integration-marketplace-presence-mapper&limit=1) [![npm version](https://img.shields.io/npm/v/@mambalabsdev/mcp-integration-marketplace-presence-mapper)](https://www.npmjs.com/package/@mambalabsdev/mcp-integration-marketplace-presence-mapper) [![npm downloads](https://img.shields.io/npm/dm/@mambalabsdev/mcp-integration-marketplace-presence-mapper)](https://www.npmjs.com/package/@mambalabsdev/mcp-integration-marketplace-presence-mapper) [![license](https://img.shields.io/github/license/mambalabsdev/mcp-integration-marketplace-presence-mapper)](https://github.com/mambalabsdev/mcp-integration-marketplace-presence-mapper/blob/main/LICENSE) [![mcpservers.org](https://img.shields.io/badge/mcpservers.org-listed-blue)](https://mcpservers.org/servers/mambalabsdev/mcp-integration-marketplace-presence-mapper)

An MCP server for the Mamba Labs Integration and Marketplace Presence Mapper actor on Apify. Give it a company domain. Each run checks ten app marketplaces and returns one row per company with the listings it found: listing URL, listing name, installs, rating, review count, and last update per marketplace.

## What's Inside

- [What it does](#what-it-does)
- [Quick start](#quick-start)
- [Prerequisites](#prerequisites)
- [Example prompts](#example-prompts)
- [Tool](#tool)
- [Inputs](#inputs)
- [Pricing](#pricing)
- [Your keys](#your-keys)
- [Full actor documentation](#full-actor-documentation)
- [License](#license)

## What it does

Find which app marketplaces list a company. Give a company domain (for example klaviyo.com), optionally its name, or a list of domains. The actor checks HubSpot App Marketplace, Salesforce AppExchange, Slack Marketplace, Zapier, Shopify App Store, Atlassian Marketplace, Chrome Web Store, Microsoft Marketplace, Google Workspace Marketplace, and the WordPress plugin directory. It reads links on the company's own site first, then matches marketplace catalogs by company name and confirms each match on the listing page. Every listing carries a status and a match confidence: 1.0 when the company site links to it, 0.9 when the listing page names the company domain, 0.7 when only the name matches. A company with no listing comes back free with `row_status` `not_listed`.

All of the work runs on Apify. This package is a thin client that starts the actor run, waits for it, and hands back the dataset rows.

## Quick start

You need Node.js 18 or newer and an Apify account with an API token.

Add this to your Claude Desktop config (`claude_desktop_config.json`), Cursor, or any MCP client:

```json
{
  "mcpServers": {
    "mamba-integration-marketplace-presence-mapper": {
      "command": "npx",
      "args": [
        "-y",
        "@mambalabsdev/mcp-integration-marketplace-presence-mapper"
      ],
      "env": {
        "APIFY_TOKEN": "your-apify-token"
      }
    }
  }
}
```

Restart the client. The `get_marketplace_presence` tool appears.

## Prerequisites

- Node.js 18 or newer.
- An Apify API token from [console.apify.com/account/integrations](https://console.apify.com/account/integrations), set as `APIFY_TOKEN`. The server lists its tool without a token; a call needs one.

## Example prompts

- "Which app marketplaces list klaviyo.com?"
- "For gong.io, outreach.io, and pandadoc.com, who is on the HubSpot and Salesforce marketplaces?"
- "Does docusign.com have a Chrome extension or a Google Workspace add-on, and how many users?"

## Tool

`get_marketplace_presence`. One tool. Example arguments:

```json
{
  "company_domain": "klaviyo.com"
}
```

## Inputs

Every buyer facing input of the live actor, unchanged. Omitted inputs take the actor's own defaults. Give one of `company_domain`, `company_name`, or `domains`.

| Input | Type | Required | Description |
|---|---|---|---|
| `company_domain` | string | no | A company website domain, for example klaviyo.com. |
| `company_name` | string | no | The name marketplaces show. Read from the homepage when omitted. |
| `domains` | array | no | Several company domains. Up to 500 per run. |
| `marketplaces` | array | no | hubspot, appexchange, slack, zapier, shopify, atlassian, chrome_web_store, microsoft, google_workspace, wordpress. Omit for all ten. |
| `max_charge_usd` | string | no | Optional cost cap for the run. |
| `skipCache` | string | no | true checks again instead of the free 7 day cache. Default false. |
| `proxy_mode` | string | no | auto, direct, or residential. Default auto. |

## Pricing

Pay per event on Apify, billed to your Apify account. Prices in USD.

| Event | Free plan | Bronze | Silver | Gold |
|---|---|---|---|---|
| `listing-found`, per marketplace listing found | $0.002 | $0.0019 | $0.0018 | $0.0017 |
| `apify-actor-start`, per run start | $0.002 | $0.002 | $0.002 | $0.002 |

A company listed on 4 marketplaces is 4 events. Companies with no listing, blocked or skipped marketplaces, and cached rows are free.

## Your keys

The actor needs no provider key. It reads public marketplace pages and sitemaps and the company's own website, and holds no key of its own. If a run uses the Apify proxy, that traffic adds nothing to your price: Mamba Labs pays the platform costs of the run. `proxy_mode` is a reliability setting.

## Full actor documentation

[apify.com/mambalabs/integration-marketplace-presence-mapper](https://apify.com/mambalabs/integration-marketplace-presence-mapper)

## License

MIT. See [LICENSE](LICENSE).
