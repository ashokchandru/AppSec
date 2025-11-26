# CI/CD Integration Guide

Integrating AppSec+ into your CI/CD pipeline ensures every build is checked for security issues.

## Supported CI Platforms
- GitHub Actions
- GitLab CI/CD
- Jenkins
- Azure DevOps
- CircleCI

## Example: GitHub Actions

```yaml
name: AppSec Scan

on: [push]

jobs:
  scan:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v3
      - name: Run AppSec+ Scan
        run: |
          curl -X POST "https://api.yourappsec/api/scans" \
            -H "Authorization: Bearer ${{ secrets.APPSEC_TOKEN }}" \
            -d '{"applicationId": "12345"}'
appsec_scan:
  script:
    - curl -X POST https://api.yourappsec/api/scans -H "Authorization: Bearer $APPSEC_TOKEN"
  only:
    - main
```
Best Practices
- Break the build on high-severity vulnerabilities
- Use environment variables for tokens
- Store scan artifacts for compliance
