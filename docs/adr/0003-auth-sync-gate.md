# ADR-0003: Authentication/state-sync feasibility gate

Status: Proposed

## Context

The desired UX requires owner-only state changes synchronized across devices, while hosting is static GitHub Pages and no external backend is allowed.

A static page cannot safely embed a confidential client secret or repository token.

## Decision

Before state-feature implementation, prototype and validate GitHub-only browser authentication/write capability.

Preferred candidate: a supported public-client GitHub authorization flow without a shipped secret.

Fallback candidate: owner-provided fine-grained PAT limited to the required repository/permission.

The accepted mechanism must be documented in a follow-up ADR after real browser testing.

## Consequences

State/UI implementation is intentionally blocked until feasibility is demonstrated. This avoids building an inbox around an authentication assumption that later fails.
