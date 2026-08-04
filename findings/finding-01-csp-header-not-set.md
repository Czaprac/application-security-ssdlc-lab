# Finding 01 - Content Security Policy Header Not Set

## Summary

OWASP ZAP reported that the target application did not set a `Content-Security-Policy` HTTP response header.

This is a security header configuration issue. A properly configured Content Security Policy can help reduce the impact of certain client-side attacks, including Cross-Site Scripting and data injection, by restricting which sources of scripts, styles, images, frames, and other resources can be loaded by the browser.

## Finding Status

Scanner finding requiring manual validation.

## Source

- Tool: OWASP ZAP
- Scan type: Baseline scan
- Target: Local OWASP Juice Shop instance
- Target URL used by ZAP: `http://host.docker.internal:3000`
- ZAP reported risk level: Medium

## Affected Areas Observed

Examples of affected areas reported by ZAP:

```text
/
 /ftp
 /ftp/eastere.gg
 /ftp/package-lock.json.bak
 /sitemap.xml
```

## Evidence

ZAP reported the absence of the `Content-Security-Policy` HTTP response header.

The finding is based on the missing security header rather than a confirmed exploit.

## Risk

Risk level used in this lab: Medium

Reasoning:

- Missing CSP does not automatically mean that the application is exploitable.
- CSP is a defense-in-depth control.
- If an XSS or content injection vulnerability exists elsewhere in the application, the lack of CSP may increase the potential impact.
- The finding should be reviewed together with the application context and other security controls.

## OWASP Mapping

### OWASP Top 10

- OWASP Top 10:2025 - A02:2025 Security Misconfiguration

Rationale:

The issue is related to missing or incomplete security-related HTTP response header configuration.

### OWASP ASVS

- OWASP ASVS v5.0.0-3.4.7

Rationale:

ASVS v5.0.0-3.4.7 is related to the `Content-Security-Policy` header and the need to define directives that limit which trusted content and resources the browser may load and execute.

## Possible Impact

If the application also contains an XSS or content injection issue, the lack of CSP may make it easier for malicious scripts or injected content to execute in the user's browser.

Potential impact may include:

- Increased impact of XSS vulnerabilities
- Increased risk from untrusted script execution
- Weaker browser-side defense-in-depth
- Reduced control over allowed content sources

## Recommended Remediation

Configure the application or web server to return a suitable `Content-Security-Policy` HTTP response header.

A starting point for a restrictive policy could include:

```text
Content-Security-Policy: default-src 'self'; object-src 'none'; base-uri 'none';
```

The final policy should be adjusted based on the application's real requirements.

Recommended actions:

- Define trusted sources for scripts, styles, images, fonts, and frames.
- Avoid wildcard sources where possible.
- Avoid unsafe inline scripts where possible.
- Test the policy in a staging or lab environment before enforcing it.
- Consider using report-only mode during tuning:

```text
Content-Security-Policy-Report-Only
```

## Validation Steps

To validate the issue manually:

1. Send an HTTP GET request to the affected endpoint.
2. Review the response headers.
3. Confirm whether the `Content-Security-Policy` header is missing.
4. If remediation is applied, repeat the request and confirm that the header is present.
5. Review whether the configured policy is restrictive enough for the application.

Example command:

```cmd
curl -I http://127.0.0.1:3000
```

## Notes

This finding was identified in a local intentionally vulnerable training application.

It should not be treated as a production security assessment result.

## References

- https://www.zaproxy.org/docs/alerts/10038/
- https://owasp.org/Top10/2025/A02_2025-Security_Misconfiguration/
- https://github.com/OWASP/ASVS/raw/v5.0.0/5.0/docs_en/OWASP_Application_Security_Verification_Standard_5.0.0_en.csv
- https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/02-Configuration_and_Deployment_Management_Testing/12-Test_for_Content_Security_Policy
