# Gentkey

**One MCP URL for all your connectors.**

Connect your accounts once at [gentkey.com](https://gentkey.com). Every AI surface — claude.ai (web & mobile), Claude Code, Cursor, any MCP client — reaches all of them through a single OAuth-protected MCP endpoint:

```
https://app.gentkey.com/mcp
```

Providers give an agent a key to the whole house. Gentkey cuts it a key to one room — and you can take it back at any time.

## Why

Connectors are a commodity; turning a coarse OAuth token into scoped, revocable, auditable capabilities is the point:

- **Capability-scoped writes.** Read tools work out of the box (switchable off per connection). Write tools require an explicit capability grant you make in the dashboard.
- **Enforced constraints.** Grants are bounded per call (e.g. *max $50 budget delta*) and per rolling window (writes / targets / dollars per hour or day).
- **Dry runs.** Every write tool advertises a `dry_run` flag that reports exactly what the real call would do — including the policy verdict, even before a grant exists — with no side effects.
- **Full audit trail.** Every decision is recorded (allowed / denied / constraint-denied / dry-run) with a human-readable plan summary — *"Change 'Summer Sale' budget: $50 → $80"*, not `grant:abc`.
- **Server-side credentials.** Upstream credentials are encrypted at rest (AES-256-GCM, AAD-bound; optional Cloud KMS envelope encryption) and injected server-side. The model never sees them.

## Connect a client

Create an account at [app.gentkey.com](https://app.gentkey.com/sign-in?mode=up), connect your services, then point your client at the endpoint. Authentication is OAuth 2.0 with dynamic client registration — no API keys to paste.

**claude.ai (web & mobile)** — Settings → Connectors → *Add custom connector* → `https://app.gentkey.com/mcp`

**Claude Code**

```sh
claude mcp add --transport http gentkey https://app.gentkey.com/mcp
```

**Cursor** — add to `~/.cursor/mcp.json`:

```json
{ "mcpServers": { "gentkey": { "url": "https://app.gentkey.com/mcp" } } }
```

Any other client that speaks Streamable HTTP with OAuth works the same way.

## What you can connect

- **Native connectors** with deep, semantic constraints: Google Ads (operation-typed capability grants, dollar caps on budget changes), Meta Ads (currency-aware budget deltas, dry-run plan/commit), BigQuery (statement-typed grants), Apple Search Ads, App Store Connect, Google Search Console, RevenueCat, and Superwall (per-resource capability maps).
- **A directory of 1,798 services** reachable by MCP passthrough, OpenAPI, or GraphQL — Notion, Linear, Slack, Stripe, Figma, Canva, Atlassian, HubSpot, Supabase, Sentry, and more. Upstream tools are proxied verbatim (schemas included) but default to write-gated: only curated read tools run without a grant, and every call is audited either way.

Full list with per-connector grant taxonomies: [docs/connectors.md](docs/connectors.md).

## About this repository

Gentkey's source is proprietary. This repository hosts the public documentation, the MCP registry manifest ([`server.json`](server.json)), and the **issue tracker** — bug reports, questions, and connector requests are welcome in [Issues](../../issues).

- [Connector catalog & grant taxonomies](docs/connectors.md)
- [Security model](docs/security.md)
- [FAQ](docs/faq.md)

- Website: [gentkey.com](https://gentkey.com)
- App: [app.gentkey.com](https://app.gentkey.com)
- MCP endpoint: `https://app.gentkey.com/mcp` (Streamable HTTP, OAuth 2.0 + dynamic client registration)
