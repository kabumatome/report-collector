# ADR-0001: GitHub-only platform boundary

Status: Accepted

## Context

The product should remain simple, inexpensive, accessible remotely, and avoid external databases/services.

## Decision

v1 platform dependencies are limited to GitHub capabilities and public source websites:
- repository,
- GitHub Actions,
- GitHub Pages,
- GitHub API,
- GitHub authentication/credentials where required.

No Supabase/Firebase/Cloudflare/custom server.

A second private GitHub repository may be used for owner state if required for privacy; this remains within the GitHub-only boundary.

## Consequences

- Static-site constraints are real and must shape authentication.
- GitHub API write limits/conflicts must be handled.
- State storage should avoid excessive commits.
- External backend conveniences are intentionally unavailable.
