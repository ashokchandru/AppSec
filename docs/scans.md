# Scans Guide

Scans evaluate applications for security issues.

## Scan Types

- **SAST** – static analysis  
- **DAST** – runtime testing  
- **SCA** – dependency vulnerabilities  
- **Container Scan** – image-level checks  

## Trigger a Scan

```bash
POST /applications/{id}/scans
```

## Check Scan Status

```bash
GET /scans/{scanId}
```

Scan lifecycle:
- queued  
- running  
- completed  
- failed  
