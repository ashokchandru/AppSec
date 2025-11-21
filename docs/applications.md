# Applications Guide

Applications represent the software systems AppSec+ analyzes.

## Create an Application

```bash
POST /applications
```

Request body:

```json
{
  "name": "Payment API",
  "criticality": "high",
  "owner": "security-team@company.com"
}
```

## Update an Application

```bash
PUT /applications/{id}
```

## Delete an Application

```bash
DELETE /applications/{id}
```

Applications serve as containers for vulnerabilities, scans, and policies.
