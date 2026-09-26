# Authentication and State Synchronization

Status: **Design gate — must be validated before owner-state implementation**

## 1. Constraint

The project must use GitHub only:
- GitHub Pages for static UI,
- GitHub repository for canonical state/data,
- GitHub API for writes,
- GitHub authentication mechanisms where technically feasible.

No external backend/database/auth provider.

## 2. Important platform constraint

GitHub Pages is static hosting. It cannot safely hold a client secret or execute a server-side OAuth token exchange.

Therefore a traditional confidential-client web OAuth flow must not be designed as if a backend exists.

## 3. Required properties

Any accepted mechanism must satisfy:
- no client secret committed or shipped to Pages,
- no repository write token embedded in built JS,
- public viewers cannot mutate owner state,
- credentials never enter repository files/logs,
- mobile and desktop are practical,
- revocation is possible,
- permission is least-privilege,
- GitHub API Contents writes can be performed with conflict handling.

## 4. Candidate A — GitHub public-client OAuth/PKCE

Preferred UX if a static browser-only implementation is demonstrably supported end-to-end.

Risk:
GitHub documentation recommends PKCE for public clients, but the documented token exchange/web behavior and browser CORS characteristics must be proven in an actual Pages-origin prototype before depending on it.

Acceptance spike:
1. Static page on a Pages-like origin initiates auth with no secret.
2. Token acquisition succeeds in supported browsers without a proxy/backend.
3. Token can read authenticated identity.
4. Token can be constrained sufficiently to this repository/write purpose.
5. Token can update a designated test state file.
6. Logout/revocation behavior is documented.
7. No token appears in repo, URL history, build output, logs, or telemetry.

If any condition fails, reject Candidate A.

## 5. Candidate B — owner-provided fine-grained PAT

Guaranteed fallback that remains GitHub-only.

Flow:
- owner creates a fine-grained PAT restricted to this repository and minimum required repository Contents permission,
- owner enters it locally on each trusted device,
- token is held only in browser-side storage chosen by the user/session policy,
- UI validates identity/repository permission before enabling mutations.

Trade-offs:
- less elegant login,
- token provisioning is manual,
- browser storage has theft/XSS implications,
- but no external service/backend is required and GitHub repository scope can be tightly limited.

This candidate must never auto-sync the credential itself. Only application state syncs.

## 6. Explicitly rejected

- hard-coded PAT/token in JavaScript,
- token stored in repository,
- client secret in Pages,
- "password" implemented only by hiding UI controls,
- public writable JSON endpoint,
- external auth proxy/service,
- assuming localStorage is secure credential storage without documenting risk.

## 7. State storage location

Owner state may reveal reading interests. Even though the report catalog is public, state is potentially personal metadata.

Before production:
- either explicitly accept public state files in this public repo,
- or store owner state in a separate **private GitHub repository** while keeping the application/public data repo public.

A second private GitHub repository still satisfies the "GitHub only" platform constraint and is the recommended privacy-preserving option if connector/auth permissions allow it.

This decision must be explicit; do not accidentally publish behavior state.

## 8. Write algorithm

1. Load canonical state + revision/blob SHA.
2. Apply local queued mutations.
3. Attempt GitHub Contents update.
4. On SHA/conflict failure, reload latest.
5. Merge mutation-by-mutation/field-by-field.
6. Retry with bounded attempts.
7. Surface failure if unresolved.

Writes are debounced/batched.

## 9. Offline behavior

The web app may queue owner mutations locally while temporarily offline.

Rules:
- queued mutations visibly indicate pending sync,
- they are retried after connectivity/auth returns,
- conflicts use the same deterministic merge logic,
- clearing browser data may lose unsynced operations; warn if pending.

## 10. Phase-0 exit criterion

No production implementation of read/bookmark/hide sync begins until one candidate is demonstrated and recorded in an ADR with:
- tested browser/device matrix,
- exact permissions,
- credential storage policy,
- state repository/publicity choice,
- threat assessment,
- recovery/revocation steps.
