# Manual HTTP Review - Burp Suite

This document summarizes a basic manual HTTP request and response review performed with Burp Suite Community Edition.

## Purpose

The purpose of this step was to manually review selected HTTP requests and responses for the local OWASP Juice Shop instance.

This review was used to confirm selected observations from the OWASP ZAP baseline scan, especially HTTP response header behavior.

## Scope

Target application:

```text
OWASP Juice Shop
```

Target URL:

```text
http://127.0.0.1:3000
```

Environment:

```text
Local Docker-based lab environment
```

## Tool Used

- Tool: Burp Suite Community Edition
- Review type: Manual HTTP request and response review
- Target: Local OWASP Juice Shop instance

## Reviewed Requests

Examples of reviewed requests:

```text
GET /
GET /robots.txt
GET /sitemap.xml
GET /api/Challenges/
GET /api/Quantitys/
```

The requests were captured in:

```text
Proxy -> HTTP history
```

## Observations

Selected HTTP responses returned:

```text
HTTP/1.1 200 OK
```

The following response header was observed:

```text
Access-Control-Allow-Origin: *
```

The reviewed response headers did not include:

```text
Content-Security-Policy
```

Other observed security-related headers included:

```text
X-Content-Type-Options: nosniff
X-Frame-Options: SAMEORIGIN
X-Recruiting: /#/jobs
```

## Relation to ZAP Findings

This manual review supported the selected DAST findings documented in this repository:

- Finding 01 - Content Security Policy Header Not Set
- Finding 02 - Cross-Domain Misconfiguration

The Burp review confirmed that selected responses included a permissive CORS header and did not include a Content Security Policy header.

## Limitations

This was a basic manual review of selected HTTP traffic.

It was not a full penetration test and did not include exploitation, authentication testing, or active attack attempts.

## Learning Outcome

This step helped practice:

- Capturing HTTP traffic with Burp Suite
- Reviewing HTTP history
- Inspecting request and response headers
- Comparing manual observations with automated DAST scanner results
- Documenting security header findings in a structured way
