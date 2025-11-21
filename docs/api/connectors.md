# Connectors API

Connectors in AppSec+ allow integration with external systems such as CI/CD pipelines, ticketing systems, code repositories, and identity providers.  
This section describes how to manage connector configurations via API.

A connector typically includes:
- type (`github`, `gitlab`, `jira`, `slack`, `jenkins`, etc.)  
- configuration fields (URL, credentials, webhook endpoints, etc.)  
- status (`active`, `inactive`, or `error`)  

---

# ListConnectors

Retrieves the list of connectors configured in the AppSec+ environment.

---

## Request Syntax

```http
GET /connectors HTTP/1.1
Host: api.appsecplus.example.com
Authorization: Bearer <access_token>
```

---

## Response Syntax

```json
[
  {
    "id": "conn-001",
    "type": "github",
    "name": "GitHub Main",
    "status": "active",
    "config": {
      "apiUrl": "https://api.github.com",
      "organization": "example-org"
    },
    "createdAt": "2025-10-01T12:34:56Z",
    "updatedAt": "2025-10-15T09:00:00Z"
  }
]
```

---

## Response Elements

| Field       | Type     | Description |
|-------------|----------|-------------|
| `id`        | string   | Connector identifier. |
| `type`      | string   | Connector type (`github`, `gitlab`, `jira`, etc.). |
| `name`      | string   | Human-readable connector name. |
| `status`    | string   | `active`, `inactive`, or `error`. |
| `config`    | object   | Provider-specific configuration object. |

---

## Multi-Language Examples

### cURL

```bash
curl -X GET "https://api.appsecplus.example.com/connectors" \
  -H "Authorization: Bearer YOUR_ACCESS_TOKEN"
```

### Python (requests)

```python
import requests

url = "https://api.appsecplus.example.com/connectors"
headers = {"Authorization": "Bearer YOUR_ACCESS_TOKEN"}

response = requests.get(url, headers=headers)
print(response.json())
```

### JavaScript (fetch)

```javascript
fetch("https://api.appsecplus.example.com/connectors", {
  method: "GET",
  headers: { "Authorization": "Bearer YOUR_ACCESS_TOKEN" }
})
  .then(res => res.json())
  .then(data => console.log(data));
```

### Java (OkHttp)

```java
OkHttpClient client = new OkHttpClient();

Request request = new Request.Builder()
    .url("https://api.appsecplus.example.com/connectors")
    .get()
    .addHeader("Authorization", "Bearer YOUR_ACCESS_TOKEN")
    .build();

Response response = client.newCall(request).execute();
System.out.println(response.body().string());
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
    req, _ := http.NewRequest("GET", "https://api.appsecplus.example.com/connectors", nil)
    req.Header.Add("Authorization", "Bearer YOUR_ACCESS_TOKEN")

    client := &http.Client{}
    resp, _ := client.Do(req)
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
| 403 | Insufficient permissions. |
| 500 | Internal server error. |

---

# RegisterConnector

Creates a new connector instance.

---

## Request Syntax

```http
POST /connectors HTTP/1.1
Host: api.appsecplus.example.com
Authorization: Bearer <access_token>
Content-Type: application/json

{
  "type": "github",
  "name": "GitHub Main",
  "config": {
    "apiUrl": "https://api.github.com",
    "token": "ghp_xxxxx",
    "organization": "example-org"
  }
}
```

---

## Request Body

| Field       | Type   | Required | Description |
|-------------|--------|----------|-------------|
| `type`      | string | Yes      | Connector type (`github`, `jira`, etc.). |
| `name`      | string | Yes      | Human-readable name. |
| `config`    | object | Yes      | Provider-specific configuration. |

---

## Response Syntax

```json
{
  "id": "conn-001",
  "type": "github",
  "name": "GitHub Main",
  "status": "active",
  "config": {
    "apiUrl": "https://api.github.com",
    "organization": "example-org"
  },
  "createdAt": "2025-10-01T12:34:56Z",
  "updatedAt": "2025-10-01T12:34:56Z"
}
```

---

## Multi-Language Examples

### cURL

```bash
curl -X POST "https://api.appsecplus.example.com/connectors" \
  -H "Authorization: Bearer YOUR_ACCESS_TOKEN" \
  -H "Content-Type: application/json" \
  -d '{
    "type": "github",
    "name": "GitHub Main",
    "config": {
      "apiUrl": "https://api.github.com",
      "token": "ghp_xxxxx",
      "organization": "example-org"
    }
  }'
```

### Python (requests)

```python
import requests

url = "https://api.appsecplus.example.com/connectors"

payload = {
    "type": "github",
    "name": "GitHub Main",
    "config": {
        "apiUrl": "https://api.github.com",
        "token": "ghp_xxxxx",
        "organization": "example-org"
    }
}

headers = {
    "Authorization": "Bearer YOUR_ACCESS_TOKEN",
    "Content-Type": "application/json"
}

response = requests.post(url, headers=headers, json=payload)
print(response.json())
```

### JavaScript (fetch)

```javascript
const payload = {
  type: "github",
  name: "GitHub Main",
  config: {
    apiUrl: "https://api.github.com",
    token: "ghp_xxxxx",
    organization: "example-org"
  }
};

fetch("https://api.appsecplus.example.com/connectors", {
  method: "POST",
  headers: {
    "Authorization": "Bearer YOUR_ACCESS_TOKEN",
    "Content-Type": "application/json"
  },
  body: JSON.stringify(payload)
})
  .then(res => res.json())
  .then(data => console.log(data));
```

### Java (OkHttp)

```java
OkHttpClient client = new OkHttpClient();
MediaType JSON = MediaType.parse("application/json; charset=utf-8");

String jsonBody = "{"
    + "\"type\":\"github\","
    + "\"name\":\"GitHub Main\","
    + "\"config\":{"
    +   "\"apiUrl\":\"https://api.github.com\","
    +   "\"token\":\"ghp_xxxxx\","
    +   "\"organization\":\"example-org\""
    + "}"
    + "}";

RequestBody body = RequestBody.create(JSON, jsonBody);

Request request = new Request.Builder()
    .url("https://api.appsecplus.example.com/connectors")
    .post(body)
    .addHeader("Authorization", "Bearer YOUR_ACCESS_TOKEN")
    .addHeader("Content-Type", "application/json")
    .build();

Response response = client.newCall(request).execute();
System.out.println(response.body().string());
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
        "type": "github",
        "name": "GitHub Main",
        "config": {
            "apiUrl": "https://api.github.com",
            "token": "ghp_xxxxx",
            "organization": "example-org"
        }
    }`)

    req, _ := http.NewRequest("POST", "https://api.appsecplus.example.com/connectors", bytes.NewBuffer(jsonStr))
    req.Header.Add("Authorization", "Bearer YOUR_ACCESS_TOKEN")
    req.Header.Add("Content-Type", "application/json")

    client := &http.Client{}
    resp, _ := client.Do(req)
    defer resp.Body.Close()

    body, _ := ioutil.ReadAll(resp.Body)
    fmt.Println(string(body))
}
```

---

## Errors

| HTTP Code | Description |
|----------|-------------|
| 400 | Invalid connector type or invalid configuration. |
| 401 | Authentication failed. |
| 403 | Insufficient permissions. |
| 409 | Connector with the same name already exists. |
| 500 | Internal server error. |

---

# GetConnector

Retrieves a single connector by ID.

---

## Request Syntax

```http
GET /connectors/{id} HTTP/1.1
Host: api.appsecplus.example.com
Authorization: Bearer <access_token>
```

---

## URI Parameters

| Name | Type   | Required | Description |
|------|--------|----------|-------------|
| `id` | string | Yes      | Connector identifier. |

---

## Response Syntax

```json
{
  "id": "conn-001",
  "type": "github",
  "name": "GitHub Main",
  "status": "active",
  "config": {
    "apiUrl": "https://api.github.com",
    "organization": "example-org"
  },
  "createdAt": "2025-10-01T12:34:56Z",
  "updatedAt": "2025-10-15T09:00:00Z"
}
```

---

## Multi-Language Examples

### cURL

```bash
curl -X GET "https://api.appsecplus.example.com/connectors/conn-001" \
  -H "Authorization: Bearer YOUR_ACCESS_TOKEN"
```

### Python (requests)

```python
import requests

conn_id = "conn-001"
url = f"https://api.appsecplus.example.com/connectors/{conn_id}"

headers = {"Authorization": "Bearer YOUR_ACCESS_TOKEN"}

response = requests.get(url, headers=headers)
print(response.json())
```

### JavaScript (fetch)

```javascript
fetch("https://api.appsecplus.example.com/connectors/conn-001", {
  method: "GET",
  headers: { "Authorization": "Bearer YOUR_ACCESS_TOKEN" }
})
  .then(res => res.json())
  .then(data => console.log(data));
```

### Java (OkHttp)

```java
OkHttpClient client = new OkHttpClient();
String connId = "conn-001";

Request request = new Request.Builder()
    .url("https://api.appsecplus.example.com/connectors/" + connId)
    .get()
    .addHeader("Authorization", "Bearer YOUR_ACCESS_TOKEN")
    .build();

Response response = client.newCall(request).execute();
System.out.println(response.body().string());
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
    connID := "conn-001"
    url := "https://api.appsecplus.example.com/connectors/" + connID

    req, _ := http.NewRequest("GET", url, nil)
    req.Header.Add("Authorization", "Bearer YOUR_ACCESS_TOKEN")

    client := &http.Client{}
    resp, _ := client.Do(req)
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
| 403 | Insufficient permissions. |
| 404 | Connector not found. |
| 500 | Internal server error. |

---

# UpdateConnector

Updates an existing connector configuration.

---

## Request Syntax

```http
PUT /connectors/{id} HTTP/1.1
Host: api.appsecplus.example.com
Authorization: Bearer <access_token>
Content-Type: application/json

{
  "name": "GitHub Updated",
  "config": {
    "apiUrl": "https://api.github.com",
    "organization": "example-org-updated"
  }
}
```

---

## URI Parameters

| Name | Type   | Required | Description |
|------|--------|----------|-------------|
| `id` | string | Yes      | Connector ID. |

---

## Multi-Language Examples

### cURL

```bash
curl -X PUT "https://api.appsecplus.example.com/connectors/conn-001" \
  -H "Authorization: Bearer YOUR_ACCESS_TOKEN" \
  -H "Content-Type: application/json" \
  -d '{
    "name": "GitHub Updated",
    "config": {
      "apiUrl": "https://api.github.com",
      "organization": "example-org-updated"
    }
  }'
```

### Python (requests)

```python
import requests

conn_id = "conn-001"
url = f"https://api.appsecplus.example.com/connectors/{conn_id}"

payload = {
    "name": "GitHub Updated",
    "config": {
        "apiUrl": "https://api.github.com",
        "organization": "example-org-updated"
    }
}

headers = {
    "Authorization": "Bearer YOUR_ACCESS_TOKEN",
    "Content-Type": "application/json"
}

response = requests.put(url, headers=headers, json=payload)
print(response.json())
```

### JavaScript (fetch)

```javascript
const connId = "conn-001";

const payload = {
  name: "GitHub Updated",
  config: {
    apiUrl: "https://api.github.com",
    organization: "example-org-updated"
  }
};

fetch(`https://api.appsecplus.example.com/connectors/${connId}`, {
  method: "PUT",
  headers: {
    "Authorization": "Bearer YOUR_ACCESS_TOKEN",
    "Content-Type": "application/json"
  },
  body: JSON.stringify(payload)
})
  .then(res => res.json())
  .then(data => console.log(data));
```

### Java (OkHttp)

```java
OkHttpClient client = new OkHttpClient();
MediaType JSON = MediaType.parse("application/json; charset=utf-8");

String connId = "conn-001";

String jsonBody = "{"
    + "\"name\":\"GitHub Updated\","
    + "\"config\":{"
    +   "\"apiUrl\":\"https://api.github.com\","
    +   "\"organization\":\"example-org-updated\""
    + "}"
    + "}";

RequestBody body = RequestBody.create(JSON, jsonBody);

Request request = new Request.Builder()
    .url("https://api.appsecplus.example.com/connectors/" + connId)
    .put(body)
    .addHeader("Authorization", "Bearer YOUR_ACCESS_TOKEN")
    .addHeader("Content-Type", "application/json")
    .build();

Response response = client.newCall(request).execute();
System.out.println(response.body().string());
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
    connID := "conn-001"
    url := "https://api.appsecplus.example.com/connectors/" + connID

    jsonStr := []byte(`{
        "name": "GitHub Updated",
        "config": {
            "apiUrl": "https://api.github.com",
            "organization": "example-org-updated"
        }
    }`)

    req, _ := http.NewRequest("PUT", url, bytes.NewBuffer(jsonStr))
    req.Header.Add("Authorization", "Bearer YOUR_ACCESS_TOKEN")
    req.Header.Add("Content-Type", "application/json")

    client := &http.Client{}
    resp, _ := client.Do(req)
    defer resp.Body.Close()

    body, _ := ioutil.ReadAll(resp.Body)
    fmt.Println(string(body))
}
```

---

## Errors

| HTTP Code | Description |
|----------|-------------|
| 400 | Invalid update fields. |
| 401 | Authentication failed. |
| 403 | Insufficient permissions. |
| 404 | Connector not found. |
| 500 | Internal server error. |

---

# DeleteConnector

Deletes a connector configuration.

---

## Request Syntax

```http
DELETE /connectors/{id} HTTP/1.1
Host: api.appsecplus.example.com
Authorization: Bearer <access_token>
```

---

## URI Parameters

| Name | Type   | Required | Description |
|------|--------|----------|-------------|
| `id` | string | Yes      | Connector identifier. |

---

## Response Syntax

Successful deletion returns **204 No Content**.

---

## Multi-Language Examples

### cURL

```bash
curl -X DELETE "https://api.appsecplus.example.com/connectors/conn-001" \
  -H "Authorization: Bearer YOUR_ACCESS_TOKEN"
```

### Python (requests)

```python
import requests

conn_id = "conn-001"
url = f"https://api.appsecplus.example.com/connectors/{conn_id}"

headers = {"Authorization": "Bearer YOUR_ACCESS_TOKEN"}

response = requests.delete(url, headers=headers)
print(response.status_code)
```

### JavaScript (fetch)

```javascript
const connId = "conn-001";

fetch(`https://api.appsecplus.example.com/connectors/${connId}`, {
  method: "DELETE",
  headers: { "Authorization": "Bearer YOUR_ACCESS_TOKEN" }
})
  .then(res => console.log(res.status));
```

### Java (OkHttp)

```java
OkHttpClient client = new OkHttpClient();
String connId = "conn-001";

Request request = new Request.Builder()
    .url("https://api.appsecplus.example.com/connectors/" + connId)
    .delete()
    .addHeader("Authorization", "Bearer YOUR_ACCESS_TOKEN")
    .build();

Response response = client.newCall(request).execute();
System.out.println(response.code());
```

### Go

```go
package main

import (
    "fmt"
    "net/http"
)

func main() {
    connID := "conn-001"
    url := "https://api.appsecplus.example.com/connectors/" + connID

    req, _ := http.NewRequest("DELETE", url, nil)
    req.Header.Add("Authorization", "Bearer YOUR_ACCESS_TOKEN")

    client := &http.Client{}
    resp, _ := client.Do(req)
    defer resp.Body.Close()

    fmt.Println(resp.StatusCode)
}
```

---

## Errors

| HTTP Code | Description |
|----------|-------------|
| 401 | Authentication failed. |
| 403 | Insufficient permissions. |
| 404 | Connector not found. |
| 500 | Internal server error. |

---

# End of connectors.md

