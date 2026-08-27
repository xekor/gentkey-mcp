# FAQ

**What is Gentkey, in one sentence?**
A hosted MCP gateway: connect your services once at
[gentkey.com](https://gentkey.com), and every MCP client reaches all of them
through one OAuth-protected URL — with scoped writes, enforced constraints,
and a full audit trail.

**How is this different from connecting MCP servers directly?**
A direct connection hands the agent whatever the OAuth token can do. Gentkey
holds the token server-side and re-exposes the service as capabilities you
grant one at a time — so an agent can change campaign budgets by at most $50
per call without also being able to delete campaigns.

**What counts as a "write"?**
Any tool call that changes state upstream. Curated connectors classify every
tool by hand; imported OpenAPI/GraphQL APIs classify from the spec (HTTP verb,
mutation vs. query); proxied MCP servers default to write-gated except for
curated read tools. Unclassifiable calls fail closed — they require the broad
write grant.

**Do denied calls reach the provider?**
No. Policy and constraint checks run in the gateway before the upstream
request is made. The denial is recorded in the audit log with the reason.

**What is a dry run?**
Every write tool accepts `dry_run: true` and returns exactly what the real
call would do — the concrete changes and the policy verdict — with no side
effects. It works even before a grant exists, so you can see what an agent
*would* do before allowing it.

**Are there approval prompts?**
No. Gentkey uses standing grants with enforced bounds instead of
interrupt-style per-call approvals. You decide once what an agent may do and
within what limits; the gateway enforces it on every call. Revoking a grant
or freezing writes takes effect on the next call.

**Can the model see my credentials?**
No. Upstream credentials are encrypted at rest and injected server-side.
They never appear in tool results or schemas.

**Which clients work?**
Anything that speaks MCP over Streamable HTTP with OAuth: claude.ai (web and
mobile), Claude Code, Cursor, and others. See the
[README](../README.md#connect-a-client) for setup snippets.

**Is Gentkey open source?**
The gateway source is proprietary. This repository hosts the public docs, the
MCP registry manifest, and the issue tracker.

**Where do I report bugs or request connectors?**
[Issues](../../../issues) on this repository.
