# Policies API

This section describes the operations used to manage security policies in AppSec+.  
Policies define rules that govern how application vulnerabilities, scan results, or risk indicators are evaluated.

A policy typically includes:
- a name  
- an action (`allow`, `block`, or `monitor`)  
- conditions referencing applications or severity levels  

---

# ListPolicies

Retrieves the list of policy resources configured in AppSec+.

---

## Request Syntax

```http
GET /policies HTTP/1.1
Host: api.appsecplus.example.com
Authorization: Bearer <access_token>
```

---

## Response Syntax

```json
[
  {
    "id": "policy-001",
    "name": "Block Critical Vulnerabilities",
    "action": "block",
    "conditions": {
      "applicationIds": ["app-12345"],
      "vulnerabilitySeverity": "critical"
    }
  }
]
```

---

## Response Elements

| Field                     | Type     | Description |
|---------------------------|----------|-------------|
| `id`                      | string   | Unique identifier of the policy. |
| `name`                    | string   | Policy name. |
| `action`                  | string   | `allow`, `block`, or `monitor`. |
| `conditions.applicationIds` | string[] | List of application IDs the policy applies to. |
| `conditions.vulnerabilitySeverity` | string | Minimum severity threshold. |

---

## Multi-Language Examples

### cURL

```bash
curl -X GET "https://api.appsecplus.example.com/policies" \
  -H "Authorization: Bearer YOUR_ACCESS_TOKEN"
```

### Python (requests)

```python
import requests

url = "https://api.appsecplus.example.com/policies"
headers = {"Authorization": "Bearer YOUR_ACCESS_TOKEN"}

response = requests.get(url, headers=headers)
print(response.json())
```

### JavaScript (fetch)

```javascript
fetch("https://api.appsecplus.example.com/policies", {
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
    .url("https://api.appsecplus.example.com/policies")
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
    req, _ := http.NewRequest("GET", "https://api.appsecplus.example.com/policies", nil)
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

# CreatePolicy

Creates a new policy resource.

---

## Request Syntax

```http
POST /policies HTTP/1.1
Host: api.appsecplus.example.com
Authorization: Bearer <access_token>
Content-Type: application/json

{
  "name": "Block Critical Vulnerabilities",
  "action": "block",
  "conditions": {
    "applicationIds": ["app-12345"],
    "vulnerabilitySeverity": "critical"
  }
}
```

---

## Request Body

| Field                         | Type     | Required | Description |
|-------------------------------|----------|----------|-------------|
| `name`                        | string   | Yes      | Policy name. |
| `action`                      | string   | Yes      | `allow`, `block`, or `monitor`. |
| `conditions.applicationIds`   | string[] | No       | Applications the policy applies to. |
| `conditions.vulnerabilitySeverity` | string | No   | Required severity threshold. |

---

## Response Syntax

```json
{
  "id": "policy-001",
  "name": "Block Critical Vulnerabilities",
  "action": "block",
  "conditions": {
    "applicationIds": ["app-12345"],
    "vulnerabilitySeverity": "critical"
  }
}
```

---

## Multi-Language Examples

### cURL

```bash
curl -X POST "https://api.appsecplus.example.com/policies" \
  -H "Authorization: Bearer YOUR_ACCESS_TOKEN" \
  -H "Content-Type: application/json" \
  -d '{
    "name": "Block Critical Vulnerabilities",
    "action": "block",
    "conditions": {
      "applicationIds": ["app-12345"],
      "vulnerabilitySeverity": "critical"
    }
  }'
```

### Python (requests)

```python
import requests

url = "https://api.appsecplus.example.com/policies"

headers = {
    "Authorization": "Bearer YOUR_ACCESS_TOKEN",
    "Content-Type": "application/json"
}

payload = {
    "name": "Block Critical Vulnerabilities",
    "action": "block",
    "conditions": {
        "applicationIds": ["app-12345"],
        "vulnerabilitySeverity": "critical"
    }
}

response = requests.post(url, headers=headers, json=payload)
print(response.json())
```

### JavaScript (fetch)

```javascript
const payload = {
  name: "Block Critical Vulnerabilities",
  action: "block",
  conditions: {
    applicationIds: ["app-12345"],
    vulnerabilitySeverity: "critical"
  }
};

fetch("https://api.appsecplus.example.com/policies", {
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
    + "\"name\":\"Block Critical Vulnerabilities\","
    + "\"action\":\"block\","
    + "\"conditions\":{"
    +   "\"applicationIds\":[\"app-12345\"],"
    +   "\"vulnerabilitySeverity\":\"critical\""
    + "}"
    + "}";

RequestBody body = RequestBody.create(JSON, jsonBody);

Request request = new Request.Builder()
    .url("https://api.appsecplus.example.com/policies")
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
        "name": "Block Critical Vulnerabilities",
        "action": "block",
        "conditions": {
            "applicationIds": ["app-12345"],
            "vulnerabilitySeverity": "critical"
        }
    }`)

    req, _ := http.NewRequest("POST", "https://api.appsecplus.example.com/policies", bytes.NewBuffer(jsonStr))
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
| 400 | Invalid action or conditions. |
| 401 | Authentication failed. |
| 403 | Insufficient permissions. |
| 409 | Policy with the same name already exists. |
| 500 | Internal server error. |

---

# GetPolicy

Retrieves a single policy by ID.

---

## Request Syntax

```http
GET /policies/{id} HTTP/1.1
Host: api.appsecplus.example.com
Authorization: Bearer <access_token>
```

---

## URI Parameters

| Name | Type   | Required | Description |
|------|--------|----------|-------------|
| `id` | string | Yes      | Unique identifier of the policy. |

---

## Response Syntax

```json
{
  "id": "policy-001",
  "name": "Block Critical Vulnerabilities",
  "action": "block",
  "conditions": {
    "applicationIds": ["app-12345"],
    "vulnerabilitySeverity": "critical"
  }
}
```

---

## Multi-Language Examples

### cURL

```bash
curl -X GET "https://api.appsecplus.example.com/policies/policy-001" \
  -H "Authorization: Bearer YOUR_ACCESS_TOKEN"
```

### Python (requests)

```python
import requests

policy_id = "policy-001"
url = f"https://api.appsecplus.example.com/policies/{policy_id}"

headers = {"Authorization": "Bearer YOUR_ACCESS_TOKEN"}

response = requests.get(url, headers=headers)
print(response.json())
```

### JavaScript (fetch)

```javascript
fetch("https://api.appsecplus.example.com/policies/policy-001", {
  method: "GET",
  headers: { "Authorization": "Bearer YOUR_ACCESS_TOKEN" }
})
  .then(res => res.json())
  .then(data => console.log(data));
```

### Java (OkHttp)

```java
OkHttpClient client = new OkHttpClient();
String policyId = "policy-001";

Request request = new Request.Builder()
    .url("https://api.appsecplus.example.com/policies/" + policyId)
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
    policyID := "policy-001"
    url := "https://api.appsecplus.example.com/policies/" + policyID

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
| 404 | Policy not found. |
| 500 | Internal server error. |

---

# UpdatePolicy

Updates an existing policy.

---

## Request Syntax

```http
PUT /policies/{id} HTTP/1.1
Host: api.appsecplus.example.com
Authorization: Bearer <access_token>
Content-Type: application/json

{
  "name": "Updated Policy Name",
  "action": "monitor",
  "conditions": {
    "applicationIds": ["app-12345"],
    "vulnerabilitySeverity": "high"
  }
}
```

---

## URI Parameters

| Name | Type   | Required | Description |
|------|--------|----------|-------------|
| `id` | string | Yes      | Unique identifier of the policy. |

---

## Multi-Language Examples

### cURL

```bash
curl -X PUT "https://api.appsecplus.example.com/policies/policy-001" \
  -H "Authorization: Bearer YOUR_ACCESS_TOKEN" \
  -H "Content-Type: application/json" \
  -d '{
    "name": "Updated Policy Name",
    "action": "monitor",
    "conditions": {
      "applicationIds": ["app-12345"],
      "vulnerabilitySeverity": "high"
    }
  }'
```

### Python (requests)

```python
import requests

policy_id = "policy-001"
url = f"https://api.appsecplus.example.com/policies/{policy_id}"

payload = {
    "name": "Updated Policy Name",
    "action": "monitor",
    "conditions": {
        "applicationIds": ["app-12345"],
        "vulnerabilitySeverity": "high"
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
const policyId = "policy-001";

const payload = {
  name: "Updated Policy Name",
  action: "monitor",
  conditions: {
    applicationIds: ["app-12345"],
    vulnerabilitySeverity: "high"
  }
};

fetch(`https://api.appsecplus.example.com/policies/${policyId}`, {
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

String policyId = "policy-001";

String jsonBody = "{"
    + "\"name\":\"Updated Policy Name\","
    + "\"action\":\"monitor\","
    + "\"conditions\":{"
    +   "\"applicationIds\":[\"app-12345\"],"
    +   "\"vulnerabilitySeverity\":\"high\""
    + "}"
    + "}";

RequestBody body = RequestBody.create(JSON, jsonBody);

Request request = new Request.Builder()
    .url("https://api.appsecplus.example.com/policies/" + policyId)
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
    policyID := "policy-001"
    url := "https://api.appsecplus.example.com/policies/" + policyID

    jsonStr := []byte(`{
        "name": "Updated Policy Name",
        "action": "monitor",
        "conditions": {
            "applicationIds": ["app-12345"],
            "vulnerabilitySeverity": "high"
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
| 400 | Invalid policy fields. |
| 401 | Authentication failed. |
| 403 | Insufficient permissions. |
| 404 | Policy not found. |
| 500 | Internal server error. |

---

# DeletePolicy

Deletes an existing policy.

---

## Request Syntax

```http
DELETE /policies/{id} HTTP/1.1
Host: api.appsecplus.example.com
Authorization: Bearer <access_token>
```

---

## URI Parameters

| Name | Type   | Required | Description |
|------|--------|----------|-------------|
| `id` | string | Yes      | Unique identifier of the policy. |

---

## Response Syntax

A successful response returns HTTP status code **204 No Content**.

---

## Multi-Language Examples

### cURL

```bash
curl -X DELETE "https://api.appsecplus.example.com/policies/policy-001" \
  -H "Authorization: Bearer YOUR_ACCESS_TOKEN"
```

### Python (requests)

```python
import requests

policy_id = "policy-001"
url = f"https://api.appsecplus.example.com/policies/{policy_id}"

headers = {"Authorization": "Bearer YOUR_ACCESS_TOKEN"}

response = requests.delete(url, headers=headers)
print(response.status_code)
```

### JavaScript (fetch)

```javascript
const policyId = "policy-001";

fetch(`https://api.appsecplus.example.com/policies/${policyId}`, {
  method: "DELETE",
  headers: { "Authorization": "Bearer YOUR_ACCESS_TOKEN" }
})
  .then(res => console.log(res.status));
```

### Java (OkHttp)

```java
OkHttpClient client = new OkHttpClient();
String policyId = "policy-001";

Request request = new Request.Builder()
    .url("https://api.appsecplus.example.com/policies/" + policyId)
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
    policyID := "policy-001"
    url := "https://api.appsecplus.example.com/policies/" + policyID

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
| 404 | Policy not found. |
| 500 | Internal server error. |

---

# End of policies.md

