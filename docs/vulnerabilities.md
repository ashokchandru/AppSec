# Vulnerability Management Guide

Vulnerabilities represent findings from your scans.

## List Vulnerabilities

```bash
GET /vulnerabilities?severity=critical
```

## Filter By
- severity  
- applicationId  
- status (`open`, `in_progress`, `resolved`, `accepted`)

## Update a Vulnerability

```bash
PATCH /vulnerabilities/{id}
{
  "status": "resolved"
}
```

## Vulnerability States
- open  
- in_progress  
- resolved  
- accepted  
