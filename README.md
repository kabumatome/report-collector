# report-collector

Public report collection and reading inbox for Japanese economic, policy, research, and institutional reports.

> Status: requirements and architecture definition in progress.

## Project principles

- GitHub-first operation: repository, Actions, Pages, Issues, and Pull Requests.
- No AI classification or summarization in the core product.
- Prefer RSS/Atom and other low-maintenance sources over site-specific scraping.
- Public repository: never commit personal information, credentials, tokens, or confidential data.
- Design before implementation: feasibility gates must be closed before feature coding starts.

## Documentation

The canonical project documentation will live under `docs/`.  
Requirements and architecture are being prepared in a design PR before implementation begins.

## Security

Never commit secrets. Report accidental exposure immediately and rotate the affected credential.
