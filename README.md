# ☁️ Cloudflare Toolkit

An [OpenClaw](https://github.com/openclaw/openclaw) skill for managing Cloudflare domains, DNS, SSL, tunnels, caching, and analytics — all from a single bash script.

**Zero dependencies** beyond `curl` and `jq`.

## Install

```bash
openclaw skills add cloudflare-toolkit
```

Or clone directly:
```bash
git clone https://github.com/InsipidPoint/openclaw-skill-cloudflare.git skills/cloudflare
```

## Setup

Set your Cloudflare API token:

```bash
export CLOUDFLARE_API_TOKEN="your-token-here"
export CLOUDFLARE_ACCOUNT_ID="your-account-id"  # optional, for tunnel operations
```

Create a token at [dash.cloudflare.com/profile/api-tokens](https://dash.cloudflare.com/profile/api-tokens). Recommended permissions:
- **Zone → Zone → Read**
- **Zone → DNS → Edit**
- **Zone → Zone Settings → Edit**
- **Zone → Cache Purge → Purge** (for cache commands)
- **Account → Cloudflare Tunnel → Edit** (for tunnel commands)

## Usage

```bash
cf <command> [args...]
```

### Zones

| Command | Description |
|---------|-------------|
| `cf zones [domain]` | List zones, optionally filter by domain |
| `cf zone-get <zone_id>` | Get zone details |
| `cf zone-id <domain>` | Get zone ID from domain name |

### DNS

| Command | Description |
|---------|-------------|
| `cf dns-list <zone_id> [type] [name]` | List DNS records |
| `cf dns-create <zone_id> <type> <name> <content> [proxied] [ttl]` | Create a record |
| `cf dns-update <zone_id> <record_id> <type> <name> <content> [proxied] [ttl]` | Update a record |
| `cf dns-delete <zone_id> <record_id>` | Delete a record |
| `cf dns-export <zone_id>` | Export all records as JSON |
| `cf dns-import <zone_id> <file.json>` | Import records from JSON |

### SSL & Settings

| Command | Description |
|---------|-------------|
| `cf ssl-get <zone_id>` | Get current SSL mode |
| `cf ssl-set <zone_id> <mode>` | Set SSL mode (off/flexible/full/strict) |
| `cf settings-list <zone_id>` | List all zone settings |
| `cf setting-get <zone_id> <setting>` | Get a specific setting |
| `cf setting-set <zone_id> <setting> <value>` | Update a setting |

### Cache

| Command | Description |
|---------|-------------|
| `cf cache-purge <zone_id> [url1 url2 ...]` | Purge specific URLs, or everything if no URLs given |

### Tunnels

| Command | Description |
|---------|-------------|
| `cf tunnels-list` | List all tunnels |
| `cf tunnel-get <tunnel_id>` | Get tunnel details |
| `cf tunnel-create <name>` | Create a new tunnel |
| `cf tunnel-delete <tunnel_id>` | Delete a tunnel |

### Firewall & Page Rules

| Command | Description |
|---------|-------------|
| `cf firewall-list <zone_id>` | List firewall rules |
| `cf pagerules-list <zone_id>` | List page rules |

### Analytics & Info

| Command | Description |
|---------|-------------|
| `cf analytics <zone_id> [since_minutes]` | Zone analytics (default: last 24h) |
| `cf verify` | Verify API token is valid |

## Examples

```bash
# Look up zone ID by domain name
cf zone-id example.com

# Add an A record (proxied)
cf dns-create abc123 A example.com 1.2.3.4 true

# Set up email
cf dns-create abc123 MX example.com "mx.provider.com" false 1
cf dns-create abc123 TXT example.com "v=spf1 include:provider.com ~all" false

# Enable strict SSL
cf ssl-set abc123 strict

# Purge entire cache
cf cache-purge abc123

# Purge specific URLs
cf cache-purge abc123 "https://example.com/style.css" "https://example.com/app.js"

# Export DNS for backup
cf dns-export abc123 > dns-backup.json

# Create a tunnel
cf tunnel-create my-tunnel
```

## How It Works

Single bash script (`scripts/cf`) wrapping the [Cloudflare API v4](https://developers.cloudflare.com/api/). No Python, no Node, no package manager — just `curl` + `jq`.

Designed for AI agents (OpenClaw, Claude Code, etc.) but works perfectly as a standalone CLI.

## License

MIT
