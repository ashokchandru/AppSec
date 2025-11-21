# Identity Groups API

Identity Groups in AppSec+ are logical collections of users, teams, or service accounts.  
They are typically used to control access, assign ownership, automate approvals, and scope policies.

Each identity group includes:
- a name  
- a description  
- a list of member identities (emails, UUIDs, or service account IDs)  
- metadata such as created/updated timestamps  

---

# ListIdentityGroups

Retrieves the list of identity groups in the AppSec+ environment.

---

## Request Syntax

```http
GET /identity-groups HTTP/1.1
Host: api.appsecplus.example.com
Authorization: Bearer <access_token>
```

---

## Response Syntax

```json
[
  {
    "id": "group-001",
    "name": "Security Team",
    "description": "Group for AppSec engineers.",
    "members": [
      "user-101",
      "user-102"
    ],
    "createdAt": "2025-10-01T12:34:56Z",
    "updatedAt": "2025-11-01T09:00:00Z"
  }
]
```

---

## Response Elements

| Field         | Type     | Description |
|---------------|----------|-------------|
| `id`          | string   | Unique group identifier. |
| `name`        | string   | Group name. |
| `description` | string   | Description of purpose. |
| `members`     | string[] | List of user or service account IDs. |

---

## Multi-Language Examples

### cURL

```bash
curl -X GET "https://api.appsecplus.example.com/identity-groups" \
  -H "Authorization: Bearer YOUR_ACCESS_TOKEN"
```

### Python (requests)

```python
import requests

url = "https://api.appsecplus.example.com/identity-groups"
headers = {"Authorization": "Bearer YOUR_ACCESS_TOKEN"}
print(requests.get(url, headers=headers).json())
```

### JavaScript (fetch)

```javascript
fetch("https://api.appsecplus.example.com/identity-groups", {
  method: "GET",
  headers: { "Authorization": "Bearer YOUR_ACCESS_TOKEN" }
})
  .then(res => res.json())
  .then(console.log);
```

### Java (OkHttp)

```java
OkHttpClient client = new OkHttpClient();

Request request = new Request.Builder()
    .url("https://api.appsecplus.example.com/identity-groups")
    .get()
    .addHeader("Authorization", "Bearer YOUR_ACCESS_TOKEN")
    .build();

System.out.println(client.newCall(request).execute().body().string());
```

### Go

```go
package main

import (
    "fmt"
    "net/http"
    "io/ioutil"
)

func main() {
    req, _ := http.NewRequest("GET", "https://api.appsecplus.example.com/identity-groups", nil)
    req.Header.Add("Authorization", "Bearer YOUR_ACCESS_TOKEN")

    resp, _ := (&http.Client{}).Do(req)
    defer resp.Body.Close()

    body, _ := ioutil.ReadAll(resp.Body)
    fmt.Println(string(body))
}
```

---

## Errors

| HTTP Code | Description |
|----------|-------------|
| 401 | Authentication failed. |
| 403 | Permission denied. |
| 500 | Internal server error. |

---

# CreateIdentityGroup

Creates a new identity group.

---

## Request Syntax

```http
POST /identity-groups HTTP/1.1
Host: api.appsecplus.example.com
Authorization: Bearer <access_token>
Content-Type: application/json

{
  "name": "Security Team",
  "description": "Group for AppSec engineers.",
  "members": ["user-101", "user-102"]
}
```

---

## Request Body

| Field         | Type     | Required | Description |
|---------------|----------|----------|-------------|
| `name`        | string   | Yes      | Group name. |
| `description` | string   | No       | Explains group purpose. |
| `members`     | string[] | No       | User or service account IDs to include. |

---

## Response Syntax

```json
{
  "id": "group-001",
  "name": "Security Team",
  "description": "Group for AppSec engineers.",
  "members": ["user-101", "user-102"],
  "createdAt": "2025-10-01T12:34:56Z",
  "updatedAt": "2025-10-01T12:34:56Z"
}
```

---

## Multi-Language Examples

### cURL

```bash
curl -X POST "https://api.appsecplus.example.com/identity-groups" \
  -H "Authorization: Bearer YOUR_ACCESS_TOKEN" \
  -H "Content-Type: application/json" \
  -d '{
    "name": "Security Team",
    "description": "Group for AppSec engineers.",
    "members": ["user-101", "user-102"]
  }'
```

### Python (requests)

```python
import requests

payload = {
    "name": "Security Team",
    "description": "Group for AppSec engineers.",
    "members": ["user-101", "user-102"]
}

print(requests.post(
    "https://api.appsecplus.example.com/identity-groups",
    headers={"Authorization": "Bearer YOUR_ACCESS_TOKEN"},
    json=payload
).json())
```

### JavaScript (fetch)

```javascript
fetch("https://api.appsecplus.example.com/identity-groups", {
  method: "POST",
  headers: {
    "Authorization": "Bearer YOUR_ACCESS_TOKEN",
    "Content-Type": "application/json"
  },
  body: JSON.stringify({
    name: "Security Team",
    description: "Group for AppSec engineers.",
    members: ["user-101", "user-102"]
  })
})
  .then(res => res.json())
  .then(console.log);
```

### Java (OkHttp)

```java
OkHttpClient client = new OkHttpClient();

String jsonBody =
    "{ \"name\":\"Security Team\","
  + "  \"description\":\"Group for AppSec engineers.\","
  + "  \"members\":[\"user-101\",\"user-102\"] }";

RequestBody body = RequestBody.create(
    MediaType.parse("application/json"), jsonBody);

Request req = new Request.Builder()
    .url("https://api.appsecplus.example.com/identity-groups")
    .post(body)
    .addHeader("Authorization", "Bearer YOUR_ACCESS_TOKEN")
    .addHeader("Content-Type", "application/json")
    .build();

System.out.println(client.newCall(req).execute().body().string());
```

### Go

```go
package main

import (
    "bytes"
    "fmt"
    "net/http"
    "io/ioutil"
)

func main() {
    jsonStr := []byte(`{
        "name": "Security Team",
        "description": "Group for AppSec engineers.",
        "members": ["user-101", "user-102"]
    }`)

    req, _ := http.NewRequest("POST",
        "https://api.appsecplus.example.com/identity-groups",
        bytes.NewBuffer(jsonStr))

    req.Header.Add("Authorization", "Bearer YOUR_ACCESS_TOKEN")
    req.Header.Add("Content-Type", "application/json")

    resp, _ := (&http.Client{}).Do(req)
    defer resp.Body.Close()

    body, _ := ioutil.ReadAll(resp.Body)
    fmt.Println(string(body))
}
```

---

## Errors

| HTTP Code | Description |
|----------|-------------|
| 400 | Invalid group name or members. |
| 401 | Authentication failed. |
| 403 | Permission denied. |
| 409 | Group with the same name exists. |
| 500 | Internal server error. |

---

# GetIdentityGroup

Retrieves details of a single identity group.

---

## Request Syntax

```http
GET /identity-groups/{id} HTTP/1.1
Host: api.appsecplus.example.com
Authorization: Bearer <access_token>
```

---

## URI Parameters

| Name | Type   | Required | Description |
|------|--------|----------|-------------|
| `id` | string | Yes      | ID of the identity group. |

---

## Response Syntax

```json
{
  "id": "group-001",
  "name": "Security Team",
  "description": "Group for AppSec engineers.",
  "members": ["user-101", "user-102"],
  "createdAt": "2025-10-01T12:34:56Z",
  "updatedAt": "2025-11-01T09:00:00Z"
}
```

---

## Multi-Language Examples

### cURL

```bash
curl -X GET "https://api.appsecplus.example.com/identity-groups/group-001" \
  -H "Authorization: Bearer YOUR_ACCESS_TOKEN"
```

### Python (requests)

```python
import requests

group_id = "group-001"
print(
    requests.get(
        f"https://api.appsecplus.example.com/identity-groups/{group_id}",
        headers={"Authorization": "Bearer YOUR_ACCESS_TOKEN"}
    ).json()
)
```

### JavaScript (fetch)

```javascript
fetch("https://api.appsecplus.example.com/identity-groups/group-001", {
  method: "GET",
  headers: { "Authorization": "Bearer YOUR_ACCESS_TOKEN" }
})
  .then(res => res.json())
  .then(console.log);
```

### Java (OkHttp)

```java
OkHttpClient client = new OkHttpClient();
String id = "group-001";

Request request = new Request.Builder()
    .url("https://api.appsecplus.example.com/identity-groups/" + id)
    .get()
    .addHeader("Authorization", "Bearer YOUR_ACCESS_TOKEN")
    .build();

System.out.println(client.newCall(request).execute().body().string());
```

### Go

```go
package main

import (
    "fmt"
    "net/http"
    "io/ioutil"
)

func main() {
    url := "https://api.appsecplus.example.com/identity-groups/group-001"

    req, _ := http.NewRequest("GET", url, nil)
    req.Header.Add("Authorization", "Bearer YOUR_ACCESS_TOKEN")

    resp, _ := (&http.Client{}).Do(req)
    defer resp.Body.Close()

    b, _ := ioutil.ReadAll(resp.Body)
    fmt.Println(string(b))
}
```

---

## Errors

| HTTP Code | Description |
|----------|-------------|
| 401 | Authentication failed. |
| 403 | Permission denied. |
| 404 | Group not found. |
| 500 | Internal server error. |

---

# UpdateIdentityGroup

Updates the name, description, or membership of a group.

---

## Request Syntax

```http
PUT /identity-groups/{id} HTTP/1.1
Host: api.appsecplus.example.com
Authorization: Bearer <access_token>
Content-Type: application/json

{
  "name": "Security Engineers",
  "description": "Updated description.",
  "members": ["user-101", "user-999"]
}
```

---

## Multi-Language Examples

### cURL

```bash
curl -X PUT "https://api.appsecplus.example.com/identity-groups/group-001" \
  -H "Authorization: Bearer YOUR_ACCESS_TOKEN" \
  -H "Content-Type: application/json" \
  -d '{
    "name": "Security Engineers",
    "description": "Updated description.",
    "members": ["user-101", "user-999"]
  }'
```

### Python (requests)

```python
import requests

payload = {
    "name": "Security Engineers",
    "description": "Updated description.",
    "members": ["user-101", "user-999"]
}

print(
    requests.put(
        "https://api.appsecplus.example.com/identity-groups/group-001",
        headers={"Authorization": "Bearer YOUR_ACCESS_TOKEN"},
        json=payload
    ).json()
)
```

### JavaScript (fetch)

```javascript
fetch("https://api.appsecplus.example.com/identity-groups/group-001", {
  method: "PUT",
  headers: {
    "Authorization": "Bearer YOUR_ACCESS_TOKEN",
    "Content-Type": "application/json"
  },
  body: JSON.stringify({
    name: "Security Engineers",
    description: "Updated description.",
    members: ["user-101", "user-999"]
  })
})
  .then(res => res.json())
  .then(console.log);
```

### Java (OkHttp)

```java
OkHttpClient client = new OkHttpClient();

String jsonBody =
    "{ \"name\":\"Security Engineers\","
  + "  \"description\":\"Updated description.\","
  + "  \"members\":[\"user-101\",\"user-999\"] }";

RequestBody body = RequestBody.create(
    MediaType.parse("application/json"), jsonBody);

Request req = new Request.Builder()
    .url("https://api.appsecplus.example.com/identity-groups/group-001")
    .put(body)
    .addHeader("Authorization", "Bearer YOUR_ACCESS_TOKEN")
    .addHeader("Content-Type", "application/json")
    .build();

System.out.println(client.newCall(req).execute().body().string());
```

### Go

```go
package main

import (
    "bytes"
    "fmt"
    "net/http"
    "io/ioutil"
)

func main() {
    jsonStr := []byte(`{
        "name": "Security Engineers",
        "description": "Updated description.",
        "members": ["user-101", "user-999"]
    }`)

    req, _ := http.NewRequest(
        "PUT",
        "https://api.appsecplus.example.com/identity-groups/group-001",
        bytes.NewBuffer(jsonStr),
    )

    req.Header.Add("Authorization", "Bearer YOUR_ACCESS_TOKEN")
    req.Header.Add("Content-Type", "application/json")

    resp, _ := (&http.Client{}).Do(req)
    defer resp.Body.Close()

    b, _ := ioutil.ReadAll(resp.Body)
    fmt.Println(string(b))
}
```

---

## Errors

| HTTP Code | Description |
|----------|-------------|
| 400 | Invalid members or fields. |
| 401 | Authentication failed. |
| 403 | Permission denied. |
| 404 | Group not found. |
| 500 | Internal server error. |

---

# DeleteIdentityGroup

Deletes a group entirely.

---

## Request Syntax

```http
DELETE /identity-groups/{id} HTTP/1.1
Host: api.appsecplus.example.com
Authorization: Bearer <access_token>
```

---

## Response

Returns **204 No Content** on success.

---

## Multi-Language Examples

### cURL

```bash
curl -X DELETE "https://api.appsecplus.example.com/identity-groups/group-001" \
  -H "Authorization: Bearer YOUR_ACCESS_TOKEN"
```

### Python (requests)

```python
import requests

print(
    requests.delete(
        "https://api.appsecplus.example.com/identity-groups/group-001",
        headers={"Authorization": "Bearer YOUR_ACCESS_TOKEN"}
    ).status_code
)
```

### JavaScript (fetch)

```javascript
fetch("https://api.appsecplus.example.com/identity-groups/group-001", {
  method: "DELETE",
  headers: { "Authorization": "Bearer YOUR_ACCESS_TOKEN" }
})
  .then(res => console.log(res.status));
```

### Java (OkHttp)

```java
OkHttpClient client = new OkHttpClient();

Request request = new Request.Builder()
    .url("https://api.appsecplus.example.com/identity-groups/group-001")
    .delete()
    .addHeader("Authorization", "Bearer YOUR_ACCESS_TOKEN")
    .build();

System.out.println(client.newCall(request).execute().code());
```

### Go

```go
package main

import (
    "fmt"
    "net/http"
)

func main() {
    req, _ := http.NewRequest(
        "DELETE",
        "https://api.appsecplus.example.com/identity-groups/group-001",
        nil,
    )

    req.Header.Add("Authorization", "Bearer YOUR_ACCESS_TOKEN")

    resp, _ := (&http.Client{}).Do(req)
    defer resp.Body.Close()

    fmt.Println(resp.StatusCode)
}
```

---

## Errors

| HTTP Code | Description |
|----------|-------------|
| 401 | Authentication failed. |
| 403 | Permission denied. |
| 404 | Group not found. |
| 500 | Internal server error. |

---

# End of identity-groups.md

