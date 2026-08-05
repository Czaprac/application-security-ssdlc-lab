# Application Security / SSDLC Lab

This repository documents my work-in-progress Application Security and SSDLC learning lab.

The goal of this lab is to practice a basic security testing workflow against a deliberately vulnerable web application in a controlled local environment.

## Status

Work in progress.

Completed so far:

- Docker Desktop installed and verified
- OWASP Juice Shop started locally as a Docker container
- Target application scope defined
- Local-only exposure confirmed
- Target application setup documented
- Basic OWASP ZAP baseline scan completed and summarized
- Two medium-risk DAST findings documented
- Remediation summary prepared for selected DAST findings
-  Manual HTTP review completed with Burp Suite
- Basic Semgrep SAST scan completed and summarized

Next planned steps:

- Document the target application setup
- Perform a basic DAST scan with OWASP ZAP
- Review selected HTTP requests manually with Burp Suite
- Perform a basic SAST scan with Semgrep
- Perform a basic SCA scan with OWASP Dependency-Check
- Document selected findings, risk, impact, and remediation
- Map selected findings to OWASP Top 10 and selected OWASP ASVS requirements
- Prepare a basic STRIDE threat model for the authentication flow
- Add a simple GitHub Actions security workflow

## Target Application

The target application used in this lab is OWASP Juice Shop.

OWASP Juice Shop is a deliberately vulnerable web application used for security training and testing.

In this lab, it is running locally in Docker.

Local URL:

`http://127.0.0.1:3000`

## Current Environment

- Host system: Windows
- Container platform: Docker Desktop
- Target application: OWASP Juice Shop
- Docker image: bkimminich/juice-shop:latest
- Container name: juice-shop
- Local port mapping: 127.0.0.1:3000 -> 3000/tcp
- Exposure: localhost only

## Documentation

- [OWASP Juice Shop Setup](target-app/juice-shop-setup.md)
- [DAST Baseline Scan - OWASP ZAP](scans/dast-zap-baseline.md)
- [Finding 01 - Content Security Policy Header Not Set](findings/finding-01-csp-header-not-set.md)
- [Finding 02 - Cross-Domain Misconfiguration](findings/finding-02-cors-misconfiguration.md)
- [Remediation Summary - DAST Findings](remediation/remediation-summary.md)
- [Manual HTTP Review - Burp Suite](scans/burp-manual-http-review.md)
- [SAST Scan - Semgrep](scans/sast-semgrep.md)
- [Finding 03 - Possible Sequelize Injection](findings/finding-03-possible-sequelize-injection.md)
- [Finding 04 - Hardcoded JWT Secret](findings/finding-04-hardcoded-jwt-secret.md)

## Lab Scope

This is a learning lab, not a production security assessment.

Testing is performed only against a local intentionally vulnerable training application.

## Data Handling

This repository does not contain:

- Real company data
- Real user data
- Passwords or credentials
- Private tokens
- Confidential information
- Production scan results

All examples and results are created in a local lab environment.
