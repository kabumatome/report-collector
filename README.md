# report-collector

Public report collection and reading inbox for Japanese economic, policy, research, and institutional reports.

> Status: requirements and architecture definition in progress.

## Product direction

- GitHub-first operation: repository, Actions, Pages, Issues, and Pull Requests.
- Feed-first ingestion: RSS/Atom before source-specific scraping.
- No AI classification or summarization in the v1 core.
- Public browsing with owner-only synchronized read/bookmark/hide state.
- Public repository: never commit personal information, credentials, tokens, or confidential data.
- Design before implementation: feasibility gates close before dependent feature coding.

## Canonical documentation

Start at **[docs/README.md](docs/README.md)**.

Key documents:

- [Product requirements](docs/requirements.md)
- [Basic architecture](docs/architecture.md)
- [Data model](docs/data-model.md)
- [Ingestion design](docs/ingestion.md)
- [Authentication and state sync](docs/auth-and-sync.md)
- [Security and privacy](docs/security-and-privacy.md)
- [Test strategy](docs/testing.md)
- [Roadmap and delivery gates](docs/roadmap.md)
- [Architecture Decision Records](docs/adr/README.md)

AI/development agents should read [AGENTS.md](AGENTS.md) before changing behavior.

## Security

This repository is public. Never commit secrets, credentials, personal data, private URLs, local machine paths, or confidential content. See [SECURITY.md](SECURITY.md).
