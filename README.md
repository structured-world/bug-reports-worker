# Bug Reports Worker

Cloudflare Worker for handling bug reports from all sw.foundation documentation sites.

## Supported Sites

| Site | Repository |
|------|------------|
| gitlab-mcp.sw.foundation | structured-world/gitlab-mcp |
| consent.sw.foundation | structured-world/consent |

To add a new site, update `SITES` config in `src/index.ts` and add route in `wrangler.toml`.

## Architecture

```
User submits bug report via widget
        |
        v
POST /api/report-bug
        |
        v
Cloudflare Worker (this repo)
        |
        v
GitHub App (sw-release-bot)
        |
        v
Creates issue in target repository
```

## Secrets Required

Set via GitHub Actions secrets or `wrangler secret put`:

| Secret | Description |
|--------|-------------|
| `CLOUDFLARE_API_TOKEN` | Cloudflare API token with Workers edit |
| `CLOUDFLARE_ACCOUNT_ID` | Cloudflare account ID |
| `SW_RELEASE_BOT_APP_ID` | GitHub App ID |
| `SW_RELEASE_BOT_PEM` | GitHub App private key (base64) |
| `SW_RELEASE_BOT_INSTALLATION_ID` | GitHub App installation ID |

## Development

```bash
npm install
npm run dev      # Local development
npm run deploy   # Manual deploy
```

## Rate Limiting

Uses KV namespace `RATE_LIMIT_KV` for IP-based rate limiting:
- 5 requests per IP per hour
- Shared across all sites
