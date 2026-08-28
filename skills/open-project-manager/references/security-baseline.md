# Security Baseline & Governance

Guidelines for maintaining strong security posture and automated scanning across repositories.

## 1. Secret Scanning & Push Protection

- Never commit credentials, tokens, private keys, API secrets, or `.env` files.
- Enable GitHub Secret Scanning and Push Protection on all public and private repositories.
- Use environment variables or designated secret managers for runtime credentials.

---

## 2. Automated Dependency Management

- Configure **Dependabot** (or Renovate) for every package ecosystem used in the repository:
  - Dockerfiles
  - GitHub Actions workflows
  - Language package managers (`package.json`, `Cargo.toml`, `pyproject.toml`, `go.mod`, etc.)
- Enable **GitHub Dependency Review** on pull requests to catch vulnerable or malicious packages prior to merge.

---

## 3. Static Code Analysis (SAST)

- Enable **CodeQL** (or language-equivalent static analyzers) in CI for supported languages.
- Run linters and type checkers as strict, blocking pre-commit or CI gates.

---

## 4. Documenting Security Exceptions

If an identified security warning or dependency update must be deferred or exempted:
1. Document the exact finding and affected component.
2. State the clear technical rationale and compensating controls.
3. Assign an owner and an expiration/re-evaluation date.
