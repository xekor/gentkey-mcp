# Security model

Gentkey sits between MCP clients and your connected services as a policy
gateway. The design goal is simple to state: **an agent can never do more
than you granted, and everything it does — or is denied — is recorded.**

## Credentials

- Upstream credentials (OAuth tokens, API keys) are encrypted at rest with
  AES-256-GCM, bound to their owning record via AAD, with optional Cloud KMS
  envelope encryption on top.
- Credentials are injected server-side on each upstream call. They are never
  returned by any tool, never appear in tool schemas, and the model never
  sees them.
- Client access to the gateway itself is OAuth 2.0 with dynamic client
  registration; connections are revocable per client at any time.

## Authorization

- **Reads by default, writes by grant.** Curated read tools work without a
  grant (switchable off per connection). Every write requires an explicit
  capability grant made in the dashboard — a standing grant, not a per-call
  approval prompt.
- **Capability-scoped, not connector-scoped.** Grants name what they govern
  in the vendor's own terms: `google-ads.write.budget`,
  `bigquery.write.dml.update`, `app-store-connect.write.pricing`.
- **Fail-closed classification.** A tool call that cannot be classified into
  a typed capability requires the broad write grant for that connector. An
  unknown operation is never treated as safe.

## Constraints

- Grants can carry enforced constraints: a maximum dollar change per call,
  and rolling-window velocity caps (writes, distinct targets, or dollars per
  hour or day).
- Constraints are enforced by arithmetic in the gateway before the upstream
  call is made — a denied call never reaches the provider. Budget deltas are
  computed across batched operations, in the ad account's currency where that
  matters.

## Dry runs and the write freeze

- Every write tool advertises a `dry_run` flag that reports exactly what the
  real call would do — including the policy verdict — with no side effects.
  Dry runs work even before a grant exists.
- A per-connection write freeze blocks all writes immediately, without
  touching individual grants.

## Audit

- Every decision is recorded: allowed, denied, constraint-denied, and dry-run
  alike, attributed to the calling agent.
- Audit entries carry a human-readable plan summary — *"Change 'Summer Sale'
  budget: $50 → $80"* — not opaque ids.

## Reporting a vulnerability

Email **hector@instasize.com** with details. Please do not open a public
issue for security reports. You'll get an acknowledgement, and a fix or a
timeline, as fast as a small team can honestly move.
