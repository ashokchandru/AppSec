# Audit Logs API

Audit Logs in AppSec+ record all security-relevant and administrative actions performed by users, service accounts, and automation systems.

Audit logs help organizations meet compliance, perform investigations, and trace who made what change and when.

Each audit log entry includes:
- actor information (user, service account, API token)  
- resource modified  
- action performed  
- timestamp  
- metadata (IP address, request ID, previous value, new value)  

---

# ListAuditLogs

Retrieves audit log entries.  
Supports filtering by actor, resource type, action, and date range.

---

## Request Syntax

```http
GET /audit-logs?actorId=string&resourceType=string&action=string&from=timestamp&to=timestamp HTTP/1.1
Host: api.appsecplus.example.com
Authorization: Bearer <access_token>
```

---

## Query Parameters

| Name           | Type   | Required | Description |
|----------------|--------|----------|-------------|
| `actorId`      | string | No       | Filter by user or service account ID. |
| `resourceType` | string | No       | Example: `application`, `policy`, `connector`, `role`. |
| `action`       | string | No       | Example: `create`, `update`, `delete`, `scan.trigger`. |
| `from`         | string | No       | ISO timestamp start boundary. |
| `to`           | string | No       | ISO timestamp end boundary. |

---

## Response Syntax

```json
[
  {
    "id": "log-001",
    "timestamp": "2025-11-01T10:30:00Z",
    "actor": {
      "id": "user-101",
      "type": "user",
      "email": "alice@example.com"
    },
    "action": "policy.update",
    "resource": {
      "type": "policy",
      "id": "policy-001",
      "name": "Block Critical Vulns"
    },
    "metadata": {
      "ip": "203.0.113.10",
      "requestId": "req-8f2f1",
      "changes": {
        "action": {
          "old": "monitor",
          "new": "block"
        }
      }
    }
  }
]
```

---

## Response Elements

| Field        | Type     | Description |
|--------------|----------|-------------|
| `id`         | string   | Identifier of the audit entry. |
| `timestamp`  | string   | UTC ISO timestamp. |
| `actor`      | object   | User/service account triggering the action. |
| `action`     | string   | Name of the action performed. |
| `resource`   | object   | Resource affected by the action. |
| `metadata`   | object   | Info such as IP, request ID, before/after values. |

---

## Multi-Language Examples

### cURL

```bash
curl -X GET "https://api.appsecplus.example.com/audit-logs?resourceType=policy" \
  -H "Authorization: Bearer YOUR_ACCESS_TOKEN"
```

### Python

```python
import requests

url = "https://api.appsecplus.example.com/audit-logs"
params = {"resourceType": "policy"}

print(requests.get(url, params=params,
  headers={"Authorization": "Bearer YOUR_ACCESS_TOKEN"}).json())
```

### JavaScript (fetch)

```javascript
const url = new URL("https://api.appsecplus.example.com/audit-logs");
url.searchParams.set("resourceType", "policy");

fetch(url, {
  headers: { "Authorization": "Bearer YOUR_ACCESS_TOKEN" }
})
  .then(r => r.json())
  .then(console.log);
```

### Java (OkHttp)

```java
HttpUrl url = HttpUrl.parse("https://api.appsecplus.example.com/audit-logs")
    .newBuilder()
    .addQueryParameter("resourceType", "policy")
    .build();

Request req = new Request.Builder()
    .url(url)
    .addHeader("Authorization", "Bearer YOUR_ACCESS_TOKEN")
    .build();

System.out.println(client.newCall(req).execute().body().string());
```

### Go

```go
package main

import (
    "fmt"
    "net/http"
    "net/url"
    "io/ioutil"
)

func main() {
    base, _ := url.Parse("https://api.appsecplus.example.com/audit-logs")
    q := base.Query()
    q.Set("resourceType", "policy")
    base.RawQuery = q.Encode()

    req, _ := http.NewRequest("GET", base.String(), nil)
    req.Header.Add("Authorization", "Bearer YOUR_ACCESS_TOKEN")

    resp, _ := (&http.Client{}).Do(req)
    defer resp.Body.Close()

    body, _ := ioutil.ReadAll(resp.Body)
    fmt.Println(string(body))
}
```

---

## Errors

| Code | Description |
|------|-------------|
| 400 | Invalid time range. |
| 401 | Authentication failed. |
| 403 | Insufficient permissions. |
| 500 | Internal server error. |

---

# GetAuditLog

Retrieves a single audit log entry by ID.

---

## Request Syntax

```http
GET /audit-logs/{id} HTTP/1.1
Host: api.appsecplus.example.com
Authorization: Bearer <access_token>
```

---

## Response Syntax

```json
{
  "id": "log-001",
  "timestamp": "2025-11-01T10:30:00Z",
  "actor": { "id": "user-101", "type": "user" },
  "action": "policy.update",
  "resource": { "type": "policy", "id": "policy-001" },
  "metadata": {
    "ip": "203.0.113.10",
    "changes": {
      "action": { "old": "monitor", "new": "block" }
    }
  }
}
```

---

## Multi-Language Examples

### cURL

```bash
curl -X GET "https://api.appsecplus.example.com/audit-logs/log-001" \
  -H "Authorization: Bearer YOUR_ACCESS_TOKEN"
```

### Python

```python
requests.get(
  "https://api.appsecplus.example.com/audit-logs/log-001",
  headers={"Authorization": "Bearer YOUR_ACCESS_TOKEN"}
).json()
```

### JavaScript

```javascript
fetch("https://api.appsecplus.example.com/audit-logs/log-001", {
  headers: { "Authorization": "Bearer YOUR_ACCESS_TOKEN" }
})
  .then(r => r.json())
  .then(console.log);
```

---

# ExportAuditLogs

Exports audit logs in JSONL or CSV format for compliance and analytics.

---

## Request Syntax

```http
POST /audit-logs/export HTTP/1.1
Host: api.appsecplus.example.com
Authorization: Bearer <access_token>
Content-Type: application/json

{
  "format": "jsonl",
  "from": "2025-10-01T00:00:00Z",
  "to": "2025-11-01T00:00:00Z"
}
```

---

## Request Body

| Field   | Type   | Required | Description |
|---------|--------|----------|-------------|
| `format`| string | Yes      | `jsonl` or `csv`. |
| `from`  | string | No       | Start timestamp. |
| `to`    | string | No       | End timestamp. |

---

## Response Syntax

```json
{
  "exportId": "exp-001",
  "status": "processing",
  "createdAt": "2025-11-02T12:00:00Z"
}
```

---

## Multi-Language Examples

### cURL

```bash
curl -X POST "https://api.appsecplus.example.com/audit-logs/export" \
  -H "Authorization: Bearer YOUR_ACCESS_TOKEN" \
  -H "Content-Type: application/json" \
  -d '{
    "format": "jsonl",
    "from": "2025-10-01T00:00:00Z",
    "to": "2025-11-01T00:00:00Z"
  }'
```

### Python

```python
payload = {
  "format": "jsonl",
  "from": "2025-10-01T00:00:00Z",
  "to": "2025-11-01T00:00:00Z"
}

requests.post(
  "https://api.appsecplus.example.com/audit-logs/export",
  headers={"Authorization": "Bearer YOUR_ACCESS_TOKEN"},
  json=payload
).json()
```

---

# GetAuditLogExport

Retrieves an export job result and download link.

---

## Request Syntax

```http
GET /audit-logs/export/{exportId} HTTP/1.1
Host: api.appsecplus.example.com
Authorization: Bearer <access_token>
```

---

## Response Syntax

```json
{
  "exportId": "exp-001",
  "status": "completed",
  "downloadUrl": "https://download.appsecplus.example.com/audit-exports/exp-001.jsonl",
  "expiresAt": "2025-11-03T00:00:00Z"
}
```

---

## Errors

| Code | Description |
|------|-------------|
| 400 | Invalid export parameters. |
| 401 | Authentication failed. |
| 403 | Insufficient permissions. |
| 404 | Export job not found. |
| 500 | Internal server error. |

---

# End of audit-logs.md

