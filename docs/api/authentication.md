# Authentication API

# Authentication

The AppSec+ API uses **OAuth 2.0 Client Credentials** for authentication.  
All requests must include a valid bearer token in the `Authorization` header.

```
Authorization: Bearer <access_token>
```

Tokens are obtained by calling the `/oauth/token` endpoint with your client ID and client secret.

---

# Obtain an Access Token

## Operation: `GetAccessToken`

Retrieves an OAuth 2.0 access token using the client credentials grant type.  
This token must be included in all subsequent API requests.

---

## Request Syntax

```
POST /oauth/token HTTP/1.1
Host: api.appsecplus.example.com
Content-Type: application/x-www-form-urlencoded

grant_type=client_credentials&
client_id=<your_client_id>&
client_secret=<your_client_secret>
```

---

## Request Parameters

| Name            | Type   | Required | Description |
|-----------------|--------|----------|-------------|
| `grant_type`    | string | Yes      | Must be `client_credentials`. |
| `client_id`     | string | Yes      | OAuth client identifier issued by AppSec+. |
| `client_secret` | string | Yes      | OAuth client secret issued by AppSec+. |

---

## Response Syntax

```json
{
  "access_token": "eyJhbGciOiJIUz...",
  "token_type": "Bearer",
  "expires_in": 3600
}
```

---

## Response Fields

| Field          | Type   | Description |
|----------------|--------|-------------|
| `access_token` | string | JWT used for authenticating API requests. |
| `token_type`   | string | Always `Bearer`. |
| `expires_in`   | int    | Token lifetime in seconds (typically 3600). |

---

# Multi-Language Examples

### **cURL**

```bash
curl -X POST "https://api.appsecplus.example.com/oauth/token" \
  -H "Content-Type: application/x-www-form-urlencoded" \
  -d "grant_type=client_credentials" \
  -d "client_id=YOUR_CLIENT_ID" \
  -d "client_secret=YOUR_CLIENT_SECRET"
```

---

### **Python (requests)**

```python
import requests

url = "https://api.appsecplus.example.com/oauth/token"
payload = {
    "grant_type": "client_credentials",
    "client_id": "YOUR_CLIENT_ID",
    "client_secret": "YOUR_CLIENT_SECRET"
}

response = requests.post(url, data=payload)
print(response.json())
```

---

### **JavaScript (fetch)**

```javascript
const params = new URLSearchParams();
params.append("grant_type", "client_credentials");
params.append("client_id", "YOUR_CLIENT_ID");
params.append("client_secret", "YOUR_CLIENT_SECRET");

fetch("https://api.appsecplus.example.com/oauth/token", {
  method: "POST",
  headers: { "Content-Type": "application/x-www-form-urlencoded" },
  body: params
})
  .then(res => res.json())
  .then(data => console.log(data));
```

---

### **Java (OkHttp)**

```java
OkHttpClient client = new OkHttpClient();

RequestBody body = new FormBody.Builder()
    .add("grant_type", "client_credentials")
    .add("client_id", "YOUR_CLIENT_ID")
    .add("client_secret", "YOUR_CLIENT_SECRET")
    .build();

Request request = new Request.Builder()
    .url("https://api.appsecplus.example.com/oauth/token")
    .post(body)
    .addHeader("Content-Type", "application/x-www-form-urlencoded")
    .build();

Response response = client.newCall(request).execute();
System.out.println(response.body().string());
```

---

### **Go**

```go
package main

import (
    "fmt"
    "net/http"
    "net/url"
    "io/ioutil"
    "strings"
)

func main() {
    data := url.Values{}
    data.Set("grant_type", "client_credentials")
    data.Set("client_id", "YOUR_CLIENT_ID")
    data.Set("client_secret", "YOUR_CLIENT_SECRET")

    req, _ := http.NewRequest("POST",
        "https://api.appsecplus.example.com/oauth/token",
        strings.NewReader(data.Encode()))

    req.Header.Add("Content-Type", "application/x-www-form-urlencoded")

    client := &http.Client{}
    resp, _ := client.Do(req)
    defer resp.Body.Close()

    body, _ := ioutil.ReadAll(resp.Body)
    fmt.Println(string(body))
}
```

---

# Error Responses

The following errors may be returned when requesting a token:

| HTTP Code | Error Code | Description |
|----------|-------------|-------------|
| **400** | `invalid_request` | Missing or invalid required parameters. |
| **401** | `invalid_client` | Invalid client ID or client secret. |
| **429** | `rate_limited` | Too many authentication attempts. Retry later. |
| **500** | `server_error` | Internal server error. |

---

# Using the Token

Once retrieved, include the token as a header:

```
Authorization: Bearer eyJhbGciOi...
```

Tokens expire after `expires_in` seconds. When expired, request a new one using the same flow.

