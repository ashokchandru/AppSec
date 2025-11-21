# AppSec+ Quickstart Guide

Welcome to AppSec+. This guide walks you through your first API workflow: authenticate, register an application, trigger a scan, and view vulnerabilities.

## 1. Get an Access Token
AppSec+ uses OAuth2 Client Credentials.

```bash
curl -X POST https://api.appsecplus.example.com/oauth/token   -H "Content-Type: application/x-www-form-urlencoded"   -d "grant_type=client_credentials"   -d "client_id=YOUR_CLIENT_ID"   -d "client_secret=YOUR_CLIENT_SECRET"
```

Use the `access_token` in all requests:

```
Authorization: Bearer <token>
```

## 2. Create an Application

```bash
curl -X POST https://api.appsecplus.example.com/applications   -H "Authorization: Bearer <token>"   -H "Content-Type: application/json"   -d '{ "name": "Inventory Service", "criticality": "high" }'
```

## 3. Run a Scan

```bash
curl -X POST https://api.appsecplus.example.com/applications/<appId>/scans   -H "Authorization: Bearer <token>"   -d '{ "scanType": "sca" }'
```

## 4. Check Scan Status

```bash
curl -X GET https://api.appsecplus.example.com/scans/<scanId>   -H "Authorization: Bearer <token>"
```

## 5. View Vulnerabilities

```bash
curl -X GET https://api.appsecplus.example.com/applications/<appId>/vulnerabilities   -H "Authorization: Bearer <token>"
```

You’ve now completed your first full AppSec+ workflow.
