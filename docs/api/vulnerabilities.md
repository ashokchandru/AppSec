# Vulnerabilities API

The Vulnerabilities API lets you list, filter, and update vulnerabilities detected by AppSec+ scans.

A **vulnerability** represents a single security finding, such as SQL injection, XSS, or insecure deserialization, associated with an application and usually originating from a specific scan.

Each vulnerability typically includes:

- `id` – unique identifier  
- `applicationId` – the affected application  
- `scanId` – the scan that found it (when applicable)  
- `severity` – `low`, `medium`, `high`, or `critical`  
- `status` – `open`, `in_progress`, `resolved`, or `accepted`  
- CWE, CVE, CVSS, and remediation metadata  

---

## ListVulnerabilities

Retrieve a list of vulnerabilities across applications with optional filters.

### HTTP request

```http
GET /vulnerabilities?applicationId=string&severity=string&status=string HTTP/1.1
Host: api.appsecplus.example.com
Authorization: Bearer <access_token>
```

### Query parameters

| Name            | Type   | Required | Description                                         |
|-----------------|--------|----------|-----------------------------------------------------|
| `applicationId` | string | No       | Filter to a specific application.                   |
| `severity`      | string | No       | `low`, `medium`, `high`, or `critical`.            |
| `status`        | string | No       | `open`, `in_progress`, `resolved`, or `accepted`.   |

### Response example

```json
[
  {
    "id": "vuln-001",
    "applicationId": "app-12345",
    "scanId": "scan-789",
    "title": "SQL Injection in login form",
    "description": "User input is concatenated directly into SQL query.",
    "severity": "critical",
    "status": "open",
    "cwe": "CWE-89",
    "cve": null,
    "cvssScore": 9.0,
    "introducedIn": "commit:abc123",
    "remediation": "Use parameterized queries.",
    "createdAt": "2025-11-01T10:00:00Z",
    "updatedAt": "2025-11-01T10:00:00Z"
  }
]
```

---

## ListApplicationVulnerabilities

```http
GET /applications/{id}/vulnerabilities
```

List vulnerabilities for a single application.

---

## GetVulnerability

```http
GET /vulnerabilities/{id}
```

Retrieve a single vulnerability by ID.

---

## UpdateVulnerabilityStatus

```http
PATCH /vulnerabilities/{id}
```

Update workflow status (`open`, `in_progress`, `resolved`, `accepted`).

---

## Error responses

Standard AppSec+ error object:

```json
{
  "code": "string",
  "message": "Human-readable message",
  "details": {}
}
```
