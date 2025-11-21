# CI/CD Integration Guide

Integrate AppSec+ into your pipeline to automate security checks.

## Step 1: Trigger a Scan

```bash
POST /applications/{id}/scans
```

## Step 2: Wait for Completion

```bash
GET /scans/{scanId}
```

## Step 3: Fail Build on Critical Issues

```bash
GET /vulnerabilities?applicationId=<id>&severity=critical&status=open
```

If results are non‑empty, fail the pipeline.

## Supported Pipelines
- GitHub Actions  
- GitLab CI  
- Jenkins  
- Azure DevOps  
