# Security Baseline & Governance

Automated repository scanning and security controls.

## Standards

1. **Secret Protection:** Never commit tokens, credentials, or `.env` files. Enable GitHub secret scanning and push protection.
2. **Dependency Management:** Configure Dependabot or Renovate for all package ecosystems. Enable Dependency Review on PRs.
3. **Static Analysis (SAST):** Run CodeQL and strict linter/typecheck gates in CI.
4. **Security Exceptions:** Document any deferred CVE with rationale, compensating controls, owner, and expiry date.
