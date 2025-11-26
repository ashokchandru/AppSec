# API Best Practices

Follow these guidelines to get the best reliability and security from the AppSec+ API.

## Use Authentication Properly
- Always use short-lived tokens  
- Rotate tokens frequently  
- Use separate tokens for different automation tasks  

## Rate Limiting
AppSec+ enforces rate limits to ensure stability:
- Limit: 100 requests/min per token  
- If rate-limited: retry with exponential backoff  

## Efficient Data Fetching
- Use pagination (`limit` & `cursor`)  
- Avoid fetching full scan history repeatedly  
- Cache static metadata  

## Error Handling

| Status | Meaning |
|--------|---------|
| 400 | Invalid request |
| 401 | Authentication failed |
| 429 | Too many requests |
| 500 | Server error (retry later) |

## Security Tips
- Do NOT embed tokens in source code  
- Use HTTPS everywhere  
- Enforce least privilege access  
