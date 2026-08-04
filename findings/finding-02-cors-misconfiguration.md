# Finding 02 - Cross-Domain Misconfiguration

## Summary

OWASP ZAP reported a Cross-Domain Misconfiguration related to Cross-Origin Resource Sharing (CORS).

The scanner observed responses containing the following HTTP response header:

```text
Access-Control-Allow-Origin: *
```

This means that affected resources allow cross-origin access from arbitrary origins.

## Finding Status

Scanner finding requiring manual validation.

## Source

- Tool: OWASP ZAP
- Scan type: Baseline scan
- Target: Local OWASP Juice Shop instance
- Target URL used by ZAP: `http://host.docker.internal:3000`
- ZAP reported risk level: Medium

## Affected Areas Observed

Examples of affected resources reported by ZAP:

```text
/chunk-5K74DZ2F.js
/chunk-PX7UKXVL.js
/chunk-VS3A3LTT.js
/robots.txt
```

## Evidence

Observed HTTP response header:

```text
Access-Control-Allow-Origin: *
```

This finding is based on a permissive CORS configuration.

## Risk

Risk level used in this lab: Medium

Reasoning:

- A wildcard CORS policy allows responses to be read by scripts from arbitrary origins.
- This is not always exploitable by itself.
- The actual risk depends on what kind of data is exposed by the affected resources.
- The risk would be higher if sensitive or user-specific data were accessible with this configuration.
- Manual validation is required to determine whether the affected responses contain sensitive information.

## OWASP Mapping

### OWASP Top 10

- OWASP Top 10:2025 - A02:2025 Security Misconfiguration

Rationale:

The issue is related to insecure or overly permissive application security configuration, specifically CORS response header configuration.

### OWASP ASVS

- OWASP ASVS v5.0.0-3.4.2

Rationale:

ASVS v5.0.0-3.4.2 verifies that the `Access-Control-Allow-Origin` header is set to a fixed trusted value or that the request `Origin` header is validated against an allowlist of trusted origins. If `Access-Control-Allow-Origin: *` is used, the response should not include sensitive information.

## Possible Impact

If sensitive unauthenticated data is exposed through affected resources, an attacker-controlled website may be able to read that data from a victim's browser.

Potential impact may include:

- Exposure of unauthenticated sensitive data
- Weakening of browser same-origin protections
- Increased risk when combined with other application issues
- Misuse of resources that were not intended to be available cross-origin

## Recommended Remediation

Review whether CORS is required for the affected resources.

Recommended actions:

- Avoid using `Access-Control-Allow-Origin: *` for sensitive resources.
- Use a strict allowlist of trusted origins.
- Do not reflect arbitrary `Origin` header values without validation.
- Confirm that resources available with wildcard CORS do not expose sensitive or user-specific data.
- Review CORS configuration separately for static files, API responses, and authenticated endpoints.

Example safer pattern:

```text
Access-Control-Allow-Origin: https://trusted.example.com
```

## Validation Steps

To validate the issue manually:

1. Send an HTTP request to an affected resource.
2. Review the response headers.
3. Confirm whether `Access-Control-Allow-Origin: *` is present.
4. Check whether the response contains sensitive or user-specific information.
5. If remediation is applied, repeat the request and confirm that the header is restricted or removed.

Example command:

```cmd
curl -I http://127.0.0.1:3000/robots.txt
```

## Manual Validation

Manual validation was performed using `curl` against the local Juice Shop instance.

Commands used:

```cmd
curl -I http://127.0.0.1:3000/
curl -I http://127.0.0.1:3000/sitemap.xml
```

Observed result:

- Both responses returned `HTTP/1.1 200 OK`.
- The following header was present in the observed response headers:

```text
Access-Control-Allow-Origin: *
```

Conclusion:

The permissive CORS configuration was manually confirmed for the tested endpoints.

## Notes

This finding was identified in a local intentionally vulnerable training application.

It should not be treated as a production security assessment result.

The finding requires manual review because permissive CORS is not always a vulnerability by itself. The real severity depends on the sensitivity of the exposed resources.

## References

- https://www.zaproxy.org/docs/alerts/10098/
- https://owasp.org/Top10/2025/A02_2025-Security_Misconfiguration/
- https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/11-Client-side_Testing/07-Testing_Cross_Origin_Resource_Sharing
- https://github.com/OWASP/ASVS/raw/v5.0.0/5.0/docs_en/OWASP_Application_Security_Verification_Standard_5.0.0_en.csv
