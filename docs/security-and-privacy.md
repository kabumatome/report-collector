# Security and Privacy

## 1. Public repository rule

Assume all committed content is permanently public and mirrored.

Never commit:
- access tokens, OAuth secrets, cookies, private keys,
- personal email/phone/address,
- local usernames/home paths,
- private repository URLs or confidential project names,
- browser exports containing credentials,
- raw HTTP headers containing Authorization/Cookie,
- downloaded content that is not clearly intended for public redistribution.

## 2. Secrets

Runtime collection should use no secrets for public RSS sources.

If a future feature needs a secret:
- store it in GitHub Actions Secrets/appropriate GitHub secret store,
- never echo it,
- never write it into generated Pages assets,
- document minimum permission and rotation.

## 3. User state privacy

Read/bookmark/hidden state may disclose personal interests.

It must not be assumed non-sensitive merely because source articles are public.

Production architecture must explicitly decide whether this state is public or held in a separate private GitHub repository.

## 4. Untrusted content

Feed fields and remote metadata are untrusted.

Requirements:
- escape/sanitize rendered text,
- never inject feed HTML directly,
- allow only safe URL schemes,
- apply noreferrer/noopener as appropriate to external links,
- prevent script execution from descriptions/titles,
- cap input sizes.

## 5. Supply chain

- lock dependencies,
- use Dependabot or equivalent GitHub-native update process when dependencies exist,
- pin Actions to trusted versions; prefer commit SHA pinning for third-party Actions,
- avoid unnecessary dependencies,
- run tests/lint/build on PRs.

## 6. Workflow permissions

Each workflow must declare least-privilege `permissions:`.

Examples:
- CI: read-only contents.
- collection write workflow: contents write only if committing generated canonical data.
- Pages deploy: only required Pages/id-token permissions.

Do not globally grant write-all.

## 7. Pull requests

PR checklist must include:
- public-data exposure review,
- secret scan/manual check,
- external-service dependency check,
- permissions change review.

## 8. Incident response

If a secret is committed:
1. revoke/rotate it immediately,
2. assume Git history/cache copies exist,
3. remove it from current content,
4. assess history rewrite only after revocation,
5. document incident without reprinting the secret.
