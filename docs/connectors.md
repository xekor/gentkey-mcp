# Connectors

Gentkey exposes every connected service through one MCP endpoint
(`https://app.gentkey.com/mcp`). Two kinds of connectors sit behind it:

**Curated connectors** carry a hand-built capability taxonomy: read tools work
without a grant, and each write is gated by a purpose-specific capability
(`google-ads.write.budget`, `app-store-connect.write.pricing`, …) that can
carry enforced dollar and velocity constraints. Each page below documents the
exact grant ids the gateway enforces.

| Connector | Auth | What it does |
|---|---|---|
| [Google Ads](https://gentkey.com/connectors/google-ads) | Google OAuth | Account discovery, campaign reporting, GAQL queries, and budget changes behind capped grants. |
| [Meta Ads](https://gentkey.com/connectors/meta-ads) | OAuth | Native campaign reporting, targeting discovery, and governed delivery and budget changes. |
| [Meta Developer Tools](https://gentkey.com/connectors/meta-developer-tools) | API key | App configuration, App Review, compliance, API usage, documentation, and governed webhook operations. |
| [Stripe](https://gentkey.com/connectors/stripe) | OAuth | Balances, charges, customers, refunds, and payment links. |
| [Notion](https://gentkey.com/connectors/notion) | OAuth | Search and update pages, databases, and comments. |
| [Linear](https://gentkey.com/connectors/linear) | OAuth | Create and update issues, projects, and cycles. |
| [GitHub](https://gentkey.com/connectors/github) | OAuth | Work with repos, issues, and pull requests. |
| [Sentry](https://gentkey.com/connectors/sentry) | OAuth | Triage issues, events, and projects. |
| [PayPal](https://gentkey.com/connectors/paypal) | OAuth | Manage orders, invoices, and subscriptions. |
| [Square](https://gentkey.com/connectors/square) | OAuth | Manage catalog, orders, and payments. |
| [Intercom](https://gentkey.com/connectors/intercom) | OAuth | Search conversations, contacts, and help articles. |
| [Webflow](https://gentkey.com/connectors/webflow) | OAuth | Edit sites, CMS collections, and pages. |
| [Asana](https://gentkey.com/connectors/asana) | OAuth | Create and update tasks, projects, and goals. |
| [Vercel](https://gentkey.com/connectors/vercel) | API key | Manage teams, projects, and deployments; search docs and inspect logs. |
| [BigQuery](https://gentkey.com/connectors/bigquery) | Google OAuth | Browse datasets and run read-only SQL; DML and DDL are granted per statement type. |
| [RevenueCat](https://gentkey.com/connectors/revenuecat) | API key | Charts, experiments, and project config reads; writes granted per resource. |
| [Superwall](https://gentkey.com/connectors/superwall) | OAuth | Browse projects, paywalls, campaigns, and charts; writes granted per resource. |
| [Hugging Face](https://gentkey.com/connectors/hugging-face) | No account needed | Search models, datasets, papers, and docs. |
| [Google Search Console](https://gentkey.com/connectors/google-search-console) | Google OAuth | Search analytics, index status, and sitemap health for verified properties. Read-only. |
| [App Store Connect](https://gentkey.com/connectors/app-store-connect) | API key | Safe App Store operations for agents: TestFlight, reviews, analytics, subscriptions, pricing, offers, localizations, screenshots, and in-app purchases. |
| [Apple Search Ads](https://gentkey.com/connectors/apple-search-ads) | API key | Campaign, ad group, and keyword management: reporting plus bid, budget, status, and keyword changes behind capped grants. |

**The directory** covers 1,798 further services reachable by remote MCP
passthrough, OpenAPI spec, or GraphQL endpoint — Slack, Figma, Canva,
Atlassian, HubSpot, Supabase, and more. Upstream tools are proxied verbatim
(schemas included) but default to write-gated: only curated read tools run
without a grant, and every call is audited either way. Any API with an
OpenAPI 3.x/2.0 spec or a GraphQL endpoint can also be imported directly;
imported operations are classified read/write from the spec itself.

Missing a connector? [Open an issue](../../../issues) — connector requests
are how the curated catalog grows.
