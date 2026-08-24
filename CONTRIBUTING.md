# Contributing

Thank you for your interest in contributing.

This is the **organization-wide default** for repositories under
[@aiperceivable](https://github.com/aiperceivable). A repository that ships its own
`CONTRIBUTING.md` overrides this file — read that one instead.

## First: which repository?

The apcore ecosystem separates *specifications* from *implementations*, and one
logical change often belongs in a repository other than the one where you noticed it.
Filing in the right place saves a round trip.

| What you want to change | Where it belongs |
|---|---|
| Protocol behavior, module schema, conformance fixtures | [apcore](https://github.com/aiperceivable/apcore) — the specification |
| A bug or API in one language SDK | `apcore-python` · `apcore-typescript` · `apcore-rust` |
| How a surface maps to the protocol (error codes, task lifecycle, argument rules) | `apcore-mcp` · `apcore-a2a` · `apcore-cli` — the surface specs |
| An adapter implementation for one language | `apcore-{mcp,a2a,cli}-{python,typescript,rust}` |
| Scanning or schema extraction shared by integrations | `apcore-toolkit` (+ its language repos) |
| A web-framework integration | `{framework}-apcore` — e.g. `fastapi-apcore`, `axum-apcore` |
| **Two languages behaving differently for the same input** | [apcore](https://github.com/aiperceivable/apcore), using the *conformance mismatch* issue template |

If you are unsure, open the issue anywhere in the organization and say so — a maintainer
will move it. A misfiled issue is much better than an unfiled one.

## Cross-language consistency

This is the constraint that makes this ecosystem different from a typical single-repo project.

The SDKs do **not** share code. They are independent implementations held together by
[conformance fixtures](https://github.com/aiperceivable/apcore/tree/main/conformance).
That means:

- **A behavioral change is not done until every affected language agrees.** A PR that
  changes observable behavior in one SDK should link tracking issues for the others, even
  if you cannot write those PRs yourself.
- **The fixtures are normative.** When an implementation and a fixture disagree, the fixture
  wins until the spec is deliberately changed. Do not "fix" an implementation by editing the
  fixture to match it.
- **Spec changes need an issue first.** Open a discussion before the PR so the cross-language
  cost is visible while the design is still cheap to change.

## Pull requests

1. Fork and branch from `main`
2. Keep the PR focused — one logical change
3. Add or update tests for anything you change
4. Run the repository's own checks before pushing (see below)
5. Update documentation when user-facing behavior changes
6. Open the PR with a description of *why*, not only *what*

Maintainers aim to respond within 7 days. The project is currently maintainer-led with a
small maintainer group — see [MAINTAINERS.md](https://github.com/aiperceivable/apcore/blob/main/MAINTAINERS.md)
and [GOVERNANCE.md](https://github.com/aiperceivable/apcore/blob/main/GOVERNANCE.md) for how
decisions are made.

### Before you push

Each repository defines its own gates; run whatever that repository documents. Typical entry points:

| Ecosystem | Usual command |
|---|---|
| Python | `pytest`, plus the repo's lint/format configuration |
| TypeScript | `npm test`, `npm run lint` |
| Rust | `cargo test --all-features`, `cargo clippy --all-targets --all-features -- -D warnings`, `cargo fmt --all -- --check` |
| Any repo with a `Makefile` | `make check` — usually mirrors CI exactly |

If CI fails on something unrelated to your change, say so in the PR rather than working
around it.

## Developer Certificate of Origin (DCO)

All contributions must be signed off:

```bash
git commit -s -m "Description of change"
```

This certifies you have the right to submit the work under the project's license.
See [developercertificate.org](https://developercertificate.org/).

## Reporting security issues

**Do not open a public issue for a security vulnerability.** See
[SECURITY.md](SECURITY.md) for the private reporting process.

## Code of Conduct

Participation is governed by the [Code of Conduct](CODE_OF_CONDUCT.md).

## License

By contributing, you agree that your contributions will be licensed under
**Apache License 2.0**.
