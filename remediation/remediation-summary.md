# Remediation Summary - DAST Findings

This document summarizes remediation recommendations for selected DAST findings identified during the OWASP ZAP baseline scan against the local OWASP Juice Shop instance.

## Scope

Target application:

```text
OWASP Juice Shop
```

Environment:

```text
Local Docker-based lab environment
```

The findings were identified and documented as part of a learning lab, not a production security assessment.

## Summary of Selected Findings

| ID | Finding | Risk | Status |
|---|---|---|---|
| Finding 01 | Content Security Policy Header Not Set | Medium | Manually validated |
| Finding 02 | Cross-Domain Misconfiguration | Medium | Manually validated |

## Finding 01 - Content Security Policy Header Not Set

### Issue

The tested responses did not include a `Content-Security-Policy` HTTP response header.

### Security Relevance

Content Security Policy is a browser-side defense-in-depth mechanism. It can reduce the potential impact of client-side attacks such as Cross-Site Scripting by limiting which sources of scripts, styles, images, frames, and other resources are allowed.

### Recommended Remediation

Configure the application or web server to return a suitable `Content-Security-Policy` header.

Example starting policy:

```text
Content-Security-Policy: default-src 'self'; object-src 'none'; base-uri 'none';
```

The final policy should be tested and adjusted based on application requirements.

### Validation After Remediation

After applying remediation:

```cmd
curl -I http://127.0.0.1:3000/
```

Confirm that the response includes a `Content-Security-Policy` header.

## Finding 02 - Cross-Domain Misconfiguration

### Issue

The tested responses included a permissive CORS header:

```text
Access-Control-Allow-Origin: *
```

### Security Relevance

A wildcard CORS policy allows responses to be accessed by scripts from arbitrary origins. This is not always exploitable by itself, but it may become risky if sensitive or user-specific data is exposed through affected resources.

### Recommended Remediation

Review whether CORS is required for the affected resources.

Recommended actions:

- Avoid using `Access-Control-Allow-Origin: *` for sensitive resources.
- Use a strict allowlist of trusted origins.
- Do not reflect arbitrary `Origin` values without validation.
- Confirm that resources exposed with wildcard CORS do not contain sensitive or user-specific information.

Example safer header:

```text
Access-Control-Allow-Origin: https://trusted.example.com
```

### Validation After Remediation

After applying remediation:

```cmd
curl -I http://127.0.0.1:3000/
```

Confirm that the CORS header is restricted, removed, or otherwise configured according to application requirements.

## Overall Notes

Both findings are configuration-related and require context-aware review.

The scan results should not be treated as confirmed production vulnerabilities without manual validation and business context.

For this lab, the main learning outcome was:

- Running a DAST baseline scan
- Reading scanner output
- Selecting relevant findings
- Manually validating HTTP response headers
- Documenting risk, impact, and remediation
