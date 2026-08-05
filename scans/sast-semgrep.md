# SAST Scan - Semgrep

This document summarizes a basic Static Application Security Testing scan performed with Semgrep against the OWASP Juice Shop source code.

## Purpose

The purpose of this step was to practice source code security scanning as part of an Application Security / SSDLC workflow.

Unlike DAST, which scans a running application, SAST reviews source code and project files.

## Scope

Target application:

```text
OWASP Juice Shop
```

Source code acquisition method:

```text
Downloaded as ZIP from the official OWASP Juice Shop GitHub repository
```

The source code was used locally only for scanning practice.

The downloaded source code was not committed to this repository.

## Tool Used

- Tool: Semgrep
- Scan type: Static Application Security Testing
- Ruleset: OWASP Top 10
- Execution method: Docker container
- Output format: JSON

## Command Used

```cmd
docker run --rm -v "%cd%:/src" -v "%USERPROFILE%\Documents\appsec-lab-reports:/reports" semgrep/semgrep semgrep scan --config p/owasp-top-ten --json --output /reports/semgrep-owasp-top-ten.json /src
```

## Scan Summary

Observed scan result:

| Metric | Result |
|---|---:|
| Findings | 32 |
| Rules run | 120 |
| Targets scanned | 1002 |
| Parsed lines | ~99.9% |

Skipped files included:

| Reason | Count |
|---|---:|
| Files larger than 1.0 MB | 2 |
| Files matching `.semgrepignore` patterns | 7 |

## Report Handling

The scan produced the following local JSON report:

```text
semgrep-owasp-top-ten.json
```

The raw JSON report was not committed to this repository.

Instead, selected findings will be reviewed manually and documented in Markdown.

## Initial Interpretation

The Semgrep scan completed successfully and identified 32 findings.

At this stage, the findings should be treated as scanner results requiring manual review.

The result count alone does not mean that all findings are confirmed vulnerabilities.

## Limitations

- This was an initial SAST scan using a predefined OWASP Top 10 ruleset.
- The scan was performed against a deliberately vulnerable training application.
- Scanner results may include false positives.
- Findings require manual review before being treated as confirmed vulnerabilities.
- The scan did not include custom Semgrep rules.

## Learning Outcome

This step helped practice:

- Running a SAST scan against source code
- Using Semgrep through Docker
- Saving scan output as JSON
- Separating raw scanner output from reviewed documentation
- Treating scanner results as findings requiring manual validation

## Next Steps

- Review the JSON report.
- Select one or more meaningful findings.
- Document selected findings with risk, impact, remediation, and OWASP mapping.
