# Webhooks & Integrations Guide

Webhooks allow your systems to react to AppSec+ events.

## Supported Events

- `scan.completed`
- `vulnerability.created`
- `policy.violated`
- `connector.statusChanged`

## Create a Webhook

```bash
POST /webhooks
```

Example:

```json
{
  "name": "Slack Alerts",
  "url": "https://hooks.slack.com/services/example",
  "events": ["scan.completed"],
  "secret": "abc123"
}
```

## Validate Signatures

Each webhook event includes an HMAC signature:

```
X-AppSecPlus-Signature
```

Your system should verify it before processing.
