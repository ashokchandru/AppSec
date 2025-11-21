# Authentication Guide

AppSec+ uses OAuth2 Client Credentials for secure, automated API access.

## Requesting Tokens

```bash
curl -X POST https://api.appsecplus.example.com/oauth/token   -H "Content-Type: application/x-www-form-urlencoded"   -d "grant_type=client_credentials"   -d "client_id=YOUR_CLIENT_ID"   -d "client_secret=YOUR_CLIENT_SECRET"
```

## Token Usage

Include the access token in every request:

```
Authorization: Bearer <access_token>
```

## Token Expiration
- Default lifetime: **1 hour**
- Request a new token when expired

## Common Authentication Errors

| Code | Meaning |
|------|---------|
| 401  | Invalid or expired token |
| 403  | Insufficient permissions |
| 429  | Too many token requests |
