# ☁️ Cloudflare Toolkit

An [OpenClaw](https://github.com/openclaw/openclaw) skill for managing Cloudflare — domains, DNS, SSL, tunnels, caching & analytics.

**Single bash script. Zero dependencies** (beyond `curl`, `jq`, `openssl`).

[![ClawHub](https://img.shields.io/badge/ClawHub-cloudflare--toolkit-4ade80?style=flat-square)](https://clawhub.com/skills/cloudflare-toolkit)

## Install

```bash
openclaw skills add cloudflare-toolkit
```

## Setup

```bash
export CLOUDFLARE_API_TOKEN="your-token"        # required
export CLOUDFLARE_ACCOUNT_ID="your-account-id"  # optional, for tunnels
```

Create a token at [dash.cloudflare.com/profile/api-tokens](https://dash.cloudflare.com/profile/api-tokens).

## What it does

| Category | Commands |
|----------|----------|
| **Zones** | list, get, lookup by domain |
| **DNS** | list, create, update, delete, export, import |
| **SSL** | get/set mode (off/flexible/full/strict) |
| **Settings** | list, get, set any zone setting |
| **Cache** | purge all or specific URLs |
| **Tunnels** | list, get, create, delete |
| **Firewall** | list rules |
| **Page Rules** | list rules |
| **Analytics** | zone stats (configurable time range) |

## Usage

```bash
# Look up zone ID
cf.sh zone-id example.com

# Add a proxied A record
cf.sh dns-create <zone_id> A example.com 1.2.3.4 true

# Enable strict SSL
cf.sh ssl-set <zone_id> strict

# Purge cache
cf.sh cache-purge <zone_id>

# Export DNS backup
cf.sh dns-export <zone_id> > backup.json
```

## How it works

Single bash script (`scripts/cf.sh`) wrapping the [Cloudflare API v4](https://developers.cloudflare.com/api/). Designed for AI agents but works as a standalone CLI.

## License

MIT
