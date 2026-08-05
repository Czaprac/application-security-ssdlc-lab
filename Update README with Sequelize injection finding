# Finding 04 - Hardcoded JWT Secret

## Summary

Semgrep reported a hardcoded JWT secret in the OWASP Juice Shop source code.

Hardcoded secrets are risky because they may be exposed through source code repositories, backups, build artifacts, logs, or accidental sharing. If a JWT signing secret is exposed and used in a real application, it may allow an attacker to forge or manipulate tokens, depending on the application design.

## Finding Status

Scanner finding requiring manual code review.

## Source

- Tool: Semgrep
- Scan type: Static Application Security Testing
- Ruleset: OWASP Top 10
- Target: OWASP Juice Shop source code
- Semgrep severity: WARNING
- Semgrep check: `javascript.jsonwebtoken.security.jwt-hardcode.hardcoded-jwt-secret`

## Affected Area Reported

Semgrep reported the finding in:

```text
/src/lib/insecurity.ts
```

Reported line:

```text
54
```

## Evidence

Semgrep reported that a hardcoded credential was detected.

Scanner message:

```text
A hard-coded credential was detected. It is not recommended to store credentials in source-code, as this risks secrets being leaked and used by either an internal or external malicious adversary.
```

## Risk

Risk level used in this lab: Medium for review priority.

Reasoning:

- The scanner reported this as a `WARNING`.
- In a real application, a hardcoded JWT secret could become high impact if it is used to sign or verify authentication tokens.
- The actual risk depends on whether the value is used in production, whether it is unique per environment, and whether it has been exposed.
- Manual code review is required before treating this as a confirmed production vulnerability.

## OWASP Mapping

### OWASP Top 10

- OWASP Top 10:2025 - A04:2025 Cryptographic Failures

Rationale:

A hardcoded JWT signing secret can be treated as a cryptographic key or secret management issue. If the secret is exposed, the cryptographic trust model around signed tokens may be weakened.

### OWASP ASVS

- OWASP ASVS v5.0.0-13.3.1

Rationale:

ASVS v5.0.0-13.3.1 verifies that a secrets management solution is used to securely create, store, control access to, and destroy backend secrets, and that secrets are not included in application source code or build artifacts.

## Possible Impact

If confirmed in a real application, this type of issue could allow attackers or unauthorized users with access to the codebase to obtain sensitive secrets.

Potential impact may include:

- Exposure of JWT signing secrets
- Token forgery or manipulation
- Authentication bypass, depending on token validation logic
- Reuse of the same secret across environments
- Difficulty rotating secrets after exposure

## Recommended Remediation

Secrets should not be hardcoded in source code.

Recommended actions:

- Store secrets outside the codebase.
- Use environment variables, a secrets manager, or a vault solution.
- Use different secrets for development, staging, and production environments.
- Rotate the exposed secret if it was ever used in a shared or production-like environment.
- Review repository history to confirm whether the secret was previously committed.
- Ensure that build artifacts do not contain embedded secrets.

Example safer pattern:

```text
JWT_SECRET is loaded from a protected environment variable or secrets manager.
```

## Manual Review Plan

To manually review this finding:

1. Open the affected file.
2. Identify the hardcoded value reported by Semgrep.
3. Check how the value is used by the application.
4. Determine whether it is used for JWT signing, verification, or another security-sensitive operation.
5. Check whether the value is test-only, development-only, or environment-specific.
6. Decide whether the finding is confirmed, partially confirmed, or acceptable for a deliberately vulnerable training application.

Prioritized file:

```text
/src/lib/insecurity.ts
```

## Notes

This finding was identified in a deliberately vulnerable training application.

The finding should be treated as a SAST result requiring manual review, not as a fully confirmed production vulnerability based only on scanner output.

## References

- https://semgrep.dev/
- https://owasp.org/Top10/2025/A04_2025-Cryptographic_Failures/
- https://owasp.org/www-project-application-security-verification-standard/
- https://cheatsheetseries.owasp.org/cheatsheets/Secrets_Management_Cheat_Sheet.html
