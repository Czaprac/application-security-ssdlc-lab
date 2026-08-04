# DAST Baseline Scan - OWASP ZAP

This document summarizes a basic DAST baseline scan performed against the local OWASP Juice Shop instance.

## Scope

Target application:

```text
OWASP Juice Shop
```

Target URL used by ZAP:

```text
http://host.docker.internal:3000
```

The application was running locally in Docker and was not exposed as a public target.

## Tool Used

- Tool: OWASP ZAP
- Scan type: Baseline scan
- ZAP version: 2.17.0
- Target: Local Docker-based OWASP Juice Shop instance

## Scan Summary

| Risk Level | Number of Alerts |
|---|---:|
| High | 0 |
| Medium | 2 |
| Low | 5 |
| Informational | 3 |

## Medium-Risk Alerts

### 1. Content Security Policy Header Not Set

Risk level: Medium

ZAP reported that the application did not set a Content Security Policy header.

A Content Security Policy can help reduce the risk of certain client-side attacks, such as Cross-Site Scripting and data injection, by limiting which sources of scripts, styles, images, frames, and other resources can be loaded by the browser.

Observed affected areas included:

- `/`
- `/ftp`
- `/sitemap.xml`

Possible remediation:

- Configure the application or web server to set a proper `Content-Security-Policy` HTTP response header.
- Start with a restrictive policy and adjust it based on application requirements.
- Avoid overly permissive sources such as unrestricted inline scripts or wildcard sources where possible.

### 2. Cross-Domain Misconfiguration

Risk level: Medium

ZAP reported a CORS-related issue where responses included:

```text
Access-Control-Allow-Origin: *
```

This means that the application allows cross-origin access from arbitrary origins for affected resources.

Observed affected areas included JavaScript files and `robots.txt`.

Possible remediation:

- Avoid using `Access-Control-Allow-Origin: *` for resources that may expose sensitive data.
- Restrict allowed origins to trusted domains only.
- Review whether CORS is needed for each affected endpoint or static resource.

## Selected Low-Risk Alerts

### Dangerous JS Functions

Risk level: Low

ZAP identified the use of a potentially dangerous JavaScript function pattern:

```text
bypassSecurityTrustHtml(
```

This finding should be manually reviewed because scanner results may require context. In some frameworks, bypassing built-in sanitization mechanisms can increase XSS risk if untrusted input reaches the function.

Possible remediation:

- Avoid bypassing framework security protections unless necessary.
- Ensure that only trusted and sanitized content is passed to such functions.
- Review the related code path manually.

### Timestamp Disclosure - Unix

Risk level: Low

ZAP identified Unix timestamp values in application responses.

This is usually low risk, but timestamps may sometimes reveal internal implementation details or be useful for correlation if combined with other information.

Possible remediation:

- Confirm whether the disclosed timestamps are expected.
- Avoid exposing unnecessary internal timestamps in public responses.

## Interpretation

This scan was a baseline DAST scan, not a full penetration test.

The results show several security headers and configuration-related findings. The most useful findings for this lab are the missing Content Security Policy header and the permissive CORS configuration, because they can be clearly documented, classified, and mapped to remediation recommendations.

## Limitations

- The scan was unauthenticated.
- The scan was performed against a local intentionally vulnerable training application.
- Results require manual review before treating them as confirmed vulnerabilities.
- The full HTML report was not committed to the repository.
