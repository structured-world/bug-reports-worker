# Bug Reports Worker

Cloudflare Worker for handling bug reports from all sw.foundation documentation sites.

## Status

**Type**: Internal tool for sw.foundation docs sites.

| Feature | Status |
|---------|--------|
| Bug report submission | Done |
| GitHub issue creation | Done |
| Rate limiting (5/hour/IP) | Done |
| Multi-site support | Done |
| GitHub Actions deploy | Done |

## Supported Sites

| Site | Repository |
|------|------------|
| gitlab-mcp.sw.foundation | structured-world/gitlab-mcp |
| privacy.sw.foundation | structured-world/vue-privacy |

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

Set via GitHub Actions secrets (org-level):

| Secret | Description |
|--------|-------------|
| `CLOUDFLARE_API_TOKEN` | Cloudflare API token with Workers edit |
| `CLOUDFLARE_ACCOUNT_ID` | Cloudflare account ID |
| `RELEASER_APP_ID` | sw-release-bot App ID |
| `RELEASER_APP_PRIVATE_KEY` | sw-release-bot private key (base64) |
| `RELEASER_APP_INSTALLATION_ID` | sw-release-bot installation ID on org |

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
