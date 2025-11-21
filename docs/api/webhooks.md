# Webhooks API

Webhooks allow external systems to receive real-time notifications when events occur in AppSec+.  
Instead of polling the API, you can subscribe webhook endpoints to specific event types.

Common webhook use cases:
- Notify Slack when a new critical vulnerability is found.  
- Trigger Jenkins pipeline when a scan completes.  
- Open a Jira ticket when a policy is violated.  
- Sync identity information with external IAM.  

Each webhook subscription includes:
- event types  
- target URL  
- secret for signature verification  
- retry policy  

---

# ListWebhooks

Retrieves all configured webhook subscriptions.

---

## Request Syntax

```http
GET /webhooks HTTP/1.1
Host: api.appsecplus.example.com
Authorization: Bearer <access_token>
```

---

## Response Syntax

```json
[
  {
    "id": "wh-001",
    "name": "Critical Vuln Slack",
    "url": "https://hooks.slack.com/123",
    "events": ["vulnerability.created"],
    "status": "active",
    "secret": "****",
    "createdAt": "2025-10-12T12:00:00Z",
    "updatedAt": "2025-11-01T10:00:00Z"
  }
]
```

---

## Response Elements

| Field     | Type       | Description |
|-----------|------------|-------------|
| `id`      | string     | Webhook ID |
| `name`    | string     | Name for identification |
| `url`     | string     | Target endpoint |
| `events`  | string[]   | Subscribed event types |
| `status`  | string     | `active`, `disabled`, `error` |
| `secret`  | string     | Masked signing secret |
| `createdAt` | string   | Timestamp |
| `updatedAt` | string   | Timestamp |

---

## Multi-Language Examples

### cURL

```bash
curl -X GET "https://api.appsecplus.example.com/webhooks" \
  -H "Authorization: Bearer YOUR_ACCESS_TOKEN"
```

### Python

```python
import requests

print(requests.get(
  "https://api.appsecplus.example.com/webhooks",
  headers={"Authorization": "Bearer YOUR_ACCESS_TOKEN"}
).json())
```

### JavaScript (fetch)

```javascript
fetch("https://api.appsecplus.example.com/webhooks", {
  headers: { "Authorization": "Bearer YOUR_ACCESS_TOKEN" }
})
  .then(r => r.json())
  .then(console.log);
```

---

# CreateWebhook

Creates a webhook subscription.

---

## Request Syntax

```http
POST /webhooks HTTP/1.1
Host: api.appsecplus.example.com
Authorization: Bearer <access_token>
Content-Type: application/json

{
  "name": "Critical Vuln Slack",
  "url": "https://hooks.slack.com/123",
  "events": ["vulnerability.created", "scan.completed"],
  "secret": "my-signing-secret"
}
```

---

## Request Body

| Field    | Type   | Required | Description |
|----------|--------|----------|-------------|
| `name`   | string | Yes      | Webhook name |
| `url`    | string | Yes      | HTTPS endpoint |
| `events` | array  | Yes      | Events to subscribe to |
| `secret` | string | Yes      | Secret for HMAC signatures |

---

## Response Syntax

```json
{
  "id": "wh-001",
  "name": "Critical Vuln Slack",
  "url": "https://hooks.slack.com/123",
  "events": ["vulnerability.created", "scan.completed"],
  "status": "active",
  "secret": "****",
  "createdAt": "2025-11-02T10:00:00Z"
}
```

---

## Multi-Language Examples

### cURL

```bash
curl -X POST "https://api.appsecplus.example.com/webhooks" \
  -H "Authorization: Bearer YOUR_ACCESS_TOKEN" \
  -H "Content-Type: application/json" \
  -d '{
    "name": "Critical Vuln Slack",
    "url": "https://hooks.slack.com/123",
    "events": ["vulnerability.created", "scan.completed"],
    "secret": "my-signing-secret"
  }'
```

### Python

```python
payload = {
  "name": "Critical Vuln Slack",
  "url": "https://hooks.slack.com/123",
  "events": ["vulnerability.created", "scan.completed"],
  "secret": "my-signing-secret"
}

requests.post(
  "https://api.appsecplus.example.com/webhooks",
  headers={"Authorization": "Bearer YOUR_ACCESS_TOKEN"},
  json=payload
).json()
```

### JavaScript (fetch)

```javascript
fetch("https://api.appsecplus.example.com/webhooks", {
  method: "POST",
  headers: {
    "Authorization": "Bearer YOUR_ACCESS_TOKEN",
    "Content-Type": "application/json"
  },
  body: JSON.stringify(payload)
})
  .then(r => r.json())
  .then(console.log);
```

---

# GetWebhook

Retrieves details of a webhook subscription.

---

## Request Syntax

```http
GET /webhooks/{id} HTTP/1.1
Host: api.appsecplus.example.com
Authorization: Bearer <access_token>
```

---

## Response Syntax

```json
{
  "id": "wh-001",
  "name": "Critical Vuln Slack",
  "url": "https://hooks.slack.com/123",
  "events": ["vulnerability.created"],
  "status": "active",
  "secret": "****",
  "createdAt": "2025-10-10T12:00:00Z",
  "updatedAt": "2025-11-01T10:00:00Z"
}
```

---

## Multi-Language Examples

### cURL

```bash
curl -X GET "https://api.appsecplus.example.com/webhooks/wh-001" \
  -H "Authorization: Bearer YOUR_ACCESS_TOKEN"
```

### Python

```python
requests.get(
  "https://api.appsecplus.example.com/webhooks/wh-001",
  headers={"Authorization": "Bearer YOUR_ACCESS_TOKEN"}
).json()
```

---

# UpdateWebhook

Updates URL, events, or secret.

---

## Request Syntax

```http
PUT /webhooks/{id} HTTP/1.1
Host: api.appsecplus.example.com
Authorization: Bearer <access_token>
Content-Type: application/json

{
  "name": "Critical Vuln Slack Updated",
  "url": "https://hooks.slack.com/new",
  "events": ["vulnerability.created"],
  "secret": "new-secret"
}
```

---

## Response Syntax

```json
{
  "id": "wh-001",
  "name": "Critical Vuln Slack Updated",
  "url": "https://hooks.slack.com/new",
  "events": ["vulnerability.created"],
  "status": "active",
  "secret": "****",
  "updatedAt": "2025-11-03T10:00:00Z"
}
```

---

# DeleteWebhook

Deletes a webhook subscription.

---

## Request Syntax

```http
DELETE /webhooks/{id} HTTP/1.1
Host: api.appsecplus.example.com
Authorization: Bearer <access_token>
```

---

## Response

Returns **204 No Content**.

---

# Webhook Event Format

All webhook POST requests sent to your URL follow the same JSON schema.

---

## Example Webhook Delivery

```json
{
  "eventId": "evt-001",
  "eventType": "vulnerability.created",
  "timestamp": "2025-11-02T12:00:00Z",
  "signature": "d1c0a9f1c230ddf0bd1f...",
  "data": {
    "id": "vuln-001",
    "severity": "critical",
    "applicationId": "app-12345",
    "title": "SQL Injection",
    "createdAt": "2025-11-02T11:59:00Z"
  }
}
```

---

# Signature Verification

Each webhook is signed using:

```
HMAC-SHA256(secret, raw_request_body)
```

Your service should verify:

1. The signature header matches.  
2. Timestamp is recent.  
3. Body is not tampered with.

---

# Retry Behavior

If your endpoint returns:

| Status | Behavior |
|--------|----------|
| 2xx    | Delivery success |
| 4xx    | Delivery dropped (client error) |
| 5xx    | Automatic retry for 24 hours with exponential backoff |

---

# End of webhooks.md

