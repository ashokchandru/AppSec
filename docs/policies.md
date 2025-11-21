# Policies Guide

Policies allow you to enforce security standards across applications.

## What You Can Enforce

- Block builds on critical severity  
- Monitor low-severity issues  
- Restrict rules to specific application groups  

## Create a Policy

```bash
POST /policies
```

Example:

```json
{
  "name": "Critical Blocker",
  "conditions": { "severity": "critical" },
  "action": "block"
}
```

## Update a Policy

```bash
PUT /policies/{id}
```
