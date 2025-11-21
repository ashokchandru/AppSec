# AppSec+ API Overview

The AppSec+ API enables programmatic access to application security resources, including applications, scans, vulnerabilities, identity groups, policies, and connectors. You can use the API to automate security workflows, integrate AppSec+ into CI/CD pipelines, and retrieve security insights at scale.

The API is organized into resource-based collections that follow REST principles. All requests use resource-specific endpoints, support standard HTTP methods, and return structured JSON responses.

---

## Base URL

All API requests are sent to the following base endpoint:

```
https://api.appsecplus.example.com
```

---

## API Authentication

AppSec+ uses **OAuth 2.0 Client Credentials** to authenticate requests.  
A valid bearer token must be included in every request.

See **authentication.md** for full details.

---

## Response Format

All successful operations return JSON-formatted responses encoded in UTF-8.

Example:

```json
{
  "id": "a1b2c3d4",
  "name": "Example Application",
  "criticality": "high"
}
```

---

## Error Handling

The AppSec+ API returns standard HTTP status codes and a structured error model.  
See **errors.md** for complete details.

---

## Available Resources

The AppSec+ API includes the following primary resource types:

- **Applications** — Register and manage application metadata.
- **Scans** — Trigger and retrieve static, dynamic, SCA, or container scans.
- **Vulnerabilities** — Retrieve vulnerability findings identified by scans.
- **Policies** — Manage policy rules used to evaluate application risk.
- **Connectors** — Manage AppSec+ connectors such as agents or scanners.
- **Identity Groups** — Manage groups used for access control or reporting.

Each resource is documented in its own section.

---

## API Versioning

The current version of the AppSec+ API is **v1**. Versioning occurs when breaking changes are introduced. New fields and optional parameters do not require a version change.

---

## Rate Limits

The AppSec+ API enforces rate limits at the tenant level to protect system stability.  
See **rate-limits.md** for details.

---

## Next Steps

To begin using the AppSec+ API:

1. Obtain OAuth credentials (client ID and secret).  
2. Request an access token.  
3. Call API endpoints with the token.  
4. Integrate into your workflows or CI/CD pipelines.

Refer to the endpoint-specific documentation for full request/response syntax.
