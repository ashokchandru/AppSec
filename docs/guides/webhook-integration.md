# Webhook Integration Guide

Webhooks allow AppSec+ to notify external systems when key events occur.

## Supported Events
- Scan started
- Scan completed
- New vulnerability discovered
- Vulnerability status changed
- Policy violated

## Creating a Webhook
1. Go to **Settings → Webhooks**
2. Click **Add Webhook**
3. Provide:
   - Endpoint URL  
   - Secret token  
   - Events to subscribe to  

## Sample Webhook Payload

```json
{
  "event": "scan.completed",
  "applicationId": "12345",
  "severity": {
    "critical": 1,
    "high": 4,
    "medium": 7
  },
  "timestamp": "2025-01-01T12:00:00Z"
}
```
Securing Webhooks
- Use HTTPS
- Validate signature hash
- Log every incoming webhook event
