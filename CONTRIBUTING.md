# Contributing to ZapoCuts

Thanks for your interest in contributing. This guide applies to all ZapoCuts repositories unless a repository's own `CONTRIBUTING.md` says otherwise.

We're a small team, so the process is intentionally lightweight. The goal is to keep changes easy to review, easy to understand later and safe to ship.

## Before you start

- For anything beyond a small fix, open an issue or start a discussion first. Agreeing on the approach early saves time for everyone.
- Check each repository's `README.md` for setup instructions, tooling and any repository-specific rules.
- Never commit secrets, credentials, API keys or personal data. If you commit one by accident, tell a maintainer right away so it can be rotated. Removing it in a later commit is not enough.
- Report security vulnerabilities privately as described in [SECURITY.md](SECURITY.md), not in a public issue.

All contributors are expected to follow our [Code of Conduct](CODE_OF_CONDUCT.md).

## Branches

Branch from `main` and use a short, descriptive name with a type prefix:

```text
<type>/<short-description>
<type>/<issue-number>-<short-description>
```

| Prefix      | Use for                                            |
| ----------- | -------------------------------------------------- |
| `feat/`     | New functionality                                  |
| `fix/`      | Bug fixes                                          |
| `refactor/` | Code changes that don't change behaviour           |
| `docs/`     | Documentation only                                 |
| `test/`     | Adding or improving tests                          |
| `chore/`    | Tooling, dependencies, build and CI configuration  |

Examples: `feat/queue-token-expiry`, `fix/142-booking-timezone`, `chore/bump-go-1-23`.

Keep branches short-lived. Rebase on or merge `main` regularly to avoid large conflicts.

## Commit messages

We follow [Conventional Commits](https://www.conventionalcommits.org/):

```text
<type>(<optional scope>): <summary>

<optional body explaining what and why>
```

- Use the same types as branch prefixes: `feat`, `fix`, `refactor`, `docs`, `test`, `chore`. Use `perf` for performance improvements.
- Write the summary in the imperative mood, in lowercase, with no trailing period, and keep it under about 72 characters.
- Use the body to explain *why* the change was made when it isn't obvious from the code.
- Mark breaking changes with `!` after the type or scope, and describe them in the body.

Examples:

```text
feat(queue): issue tokens on shop check-in
fix(booking): reject appointments outside shop hours
refactor(auth)!: move token validation into shared middleware
```

Pull requests are usually squash-merged, so the PR title becomes the final commit message. Write PR titles in the same format.

## Pull requests

- **Keep them small and focused.** One logical change per PR. Large changes are easier to review when split into a series of smaller PRs.
- **Describe the change.** Fill in the pull request template: what changed, why and how you tested it. Link the related issue if there is one.
- **Open drafts early** if you want feedback on direction before the work is finished.
- **Make sure checks pass** before requesting review.
- **Include screenshots or recordings** for user-facing UI changes.
- **Call out risk** such as database migrations, API or `.proto` changes, new dependencies or configuration changes.

## Code review

Every change to `main` goes through a pull request and needs at least one approval from another team member.

**As an author**

- Review your own diff before requesting review.
- Respond to every comment, even if only to acknowledge it. Resolve threads once they're addressed.
- If a discussion goes back and forth, move to a quick call and summarise the outcome in the PR.

**As a reviewer**

- Aim to review within one working day.
- Focus first on correctness, security, data handling and clarity, then on style.
- Be specific and kind. Explain the reasoning behind requests.
- Prefix optional suggestions with `nit:` so the author knows they don't block merging.
- Approve when the change is good enough to ship. It doesn't need to be how you would have written it.

## Testing

- New behaviour should come with tests. Bug fixes should include a test that fails without the fix where practical.
- Run the repository's test suite locally before opening a PR. For Go services, that usually means `go test ./...`, plus `-race` for concurrent code.
- Prefer fast, deterministic tests. Avoid tests that depend on timing, external networks or execution order.
- Test the edge cases that matter for this product: time zones, shop opening hours, concurrent bookings and queue updates.
- If something can't reasonably be tested automatically, describe how you verified it manually in the PR.

## Documentation

- Update the `README.md` when setup steps, configuration or commands change.
- Document public APIs. For gRPC services, comment messages, fields and RPCs in `.proto` files.
- Use code comments to explain *why*, not *what*. The code should make the *what* clear.
- Record significant technical decisions in the repository's `docs/` directory, if it has one, with a short note on context and alternatives considered.

## Code quality

- **Format and lint.** Use the formatters and linters each repository has configured (for example `gofmt` for Go). CI enforces them.
- **Favour readability.** Clear names, small functions and straightforward control flow are better than clever code.
- **Handle errors deliberately.** Don't ignore errors. Add context when returning them, and don't leak internal details to clients.
- **Keep changes backward compatible.** Database migrations, `.proto` definitions and public APIs must not break existing clients or running services. In Protocol Buffers, never reuse or renumber fields; reserve removed ones instead.
- **Be careful with dependencies.** Add new ones only when they clearly earn their place, and mention them in the PR.
- **Protect user data.** Don't log personal information such as phone numbers, names or locations. Validate all input at service boundaries.

## Questions

If something in this guide is unclear or doesn't fit the way we work, open an issue or raise it with the team. This document should change as we do.
