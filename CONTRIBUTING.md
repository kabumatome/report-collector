# Contributing

## Before coding

Read `AGENTS.md` and the relevant canonical documents in `docs/`.

Use an Issue for non-trivial work. Requirements/design changes should be reviewed before implementation when they alter contracts.

## Branch / PR

Prefer focused branches such as:
- `feat/...`
- `fix/...`
- `design/...`
- `chore/...`

PRs should:
- reference an Issue when applicable,
- explain user-visible and architectural impact,
- include tests,
- update docs/contracts,
- state security/privacy impact.

## Public repository safety

Do not use real credentials or private data in fixtures/examples. Use synthetic placeholders.

## Commit hygiene

Keep generated runtime data out of design/code commits unless the change explicitly concerns that data.
