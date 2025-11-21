# Applications API

The operations in this section enable you to create, retrieve, update, delete, and scan application resources in AppSec+. Use these operations to manage your application inventory and initiate security scans programmatically.

# ListApplications

Lists the application resources configured in the AppSec+ environment.  
You can filter the results by owner, criticality, or tag.

---

## Request Syntax

```http
GET /applications?owner=string&criticality=string&tag=string HTTP/1.1
Host: api.appsecplus.example.com
Authorization: Bearer <access_token>
```

---

## Query Parameters

| Name          | Type   | Required | Description |
|---------------|--------|----------|-------------|
| `owner`       | string | No       | Filters applications by owner identifier. |
| `criticality` | string | No       | Filters by criticality. Valid values are `low`, `medium`, `high`, and `critical`. |
| `tag`         | string | No       | Filters applications that contain the specified tag. |

---

## Response Syntax

```json
[
  {
    "id": "app-12345",
    "name": "Payments API",
    "description": "Handles payment workflows.",
    "criticality": "high",
    "owner": "team-payments",
    "repoUrl": "https://github.com/example/payments",
    "serviceUrl": "https://payments.example.com",
    "language": "java",
    "tags": ["payments", "pci"],
    "createdAt": "2025-10-01T12:34:56Z",
    "updatedAt": "2025-11-01T08:30:00Z"
  }
]
```

---

## Response Elements

The response is an array of `Application` objects.

Key fields include:

| Field          | Type     | Description |
|----------------|----------|-------------|
| `id`           | string   | Unique identifier of the application. |
| `name`         | string   | Human-readable application name. |
| `description`  | string   | Description of the application's purpose. |
| `criticality`  | string   | Business criticality: `low`, `medium`, `high`, or `critical`. |
| `owner`        | string   | Owner or owning team of the application. |
| `repoUrl`      | string   | Source code repository URL. |
| `serviceUrl`   | string   | Base URL of the deployed service (if applicable). |
| `language`     | string   | Primary implementation language. |
| `tags`         | string[] | Arbitrary tags associated with the application. |

---

## Multi-Language Examples

### cURL

```bash
curl -X GET "https://api.appsecplus.example.com/applications?criticality=high" \
  -H "Authorization: Bearer YOUR_ACCESS_TOKEN"
```

### Python (requests)

```python
import requests

url = "https://api.appsecplus.example.com/applications"
params = {"criticality": "high"}

headers = {
    "Authorization": "Bearer YOUR_ACCESS_TOKEN"
}

response = requests.get(url, headers=headers, params=params)
print(response.json())
```

### JavaScript (fetch)

```javascript
const url = new URL("https://api.appsecplus.example.com/applications");
url.searchParams.set("criticality", "high");

fetch(url, {
  method: "GET",
  headers: {
    "Authorization": "Bearer YOUR_ACCESS_TOKEN"
  }
})
  .then(res => res.json())
  .then(data => console.log(data));
```

### Java (OkHttp)

```java
OkHttpClient client = new OkHttpClient();

HttpUrl url = HttpUrl.parse("https://api.appsecplus.example.com/applications")
    .newBuilder()
    .addQueryParameter("criticality", "high")
    .build();

Request request = new Request.Builder()
    .url(url)
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
    "net/url"
    "io/ioutil"
)

func main() {
    base, _ := url.Parse("https://api.appsecplus.example.com/applications")
    params := url.Values{}
    params.Set("criticality", "high")
    base.RawQuery = params.Encode()

    req, _ := http.NewRequest("GET", base.String(), nil)
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

| HTTP Code | Description                      |
|----------|----------------------------------|
| 401      | Authentication failed.           |
| 403      | Insufficient permissions.        |
| 429      | Too many requests.               |
| 500      | Internal server error.           |

---

# CreateApplication

Creates a new application resource in AppSec+.  
Use this operation to register a new application in the inventory.

---

## Request Syntax

```http
POST /applications HTTP/1.1
Host: api.appsecplus.example.com
Authorization: Bearer <access_token>
Content-Type: application/json

{
  "name": "Payments API",
  "description": "Handles payment workflows.",
  "criticality": "high",
  "owner": "team-payments",
  "repoUrl": "https://github.com/example/payments",
  "serviceUrl": "https://payments.example.com",
  "language": "java",
  "tags": ["payments", "pci"]
}
```

---

## Request Body

The request body is an `Application` object.

Required fields:

| Field  | Type   | Description |
|--------|--------|-------------|
| `name` | string | Name of the application. |

Optional fields:

| Field          | Type     | Description |
|----------------|----------|-------------|
| `description`  | string   | Description of the application. |
| `criticality`  | string   | `low`, `medium`, `high`, or `critical`. Defaults to `medium`. |
| `owner`        | string   | Application owner or team. |
| `repoUrl`      | string   | Repository URL. |
| `serviceUrl`   | string   | Base service URL. |
| `language`     | string   | Primary language. |
| `tags`         | string[] | Tags associated with the application. |

---

## Response Syntax

```json
{
  "id": "app-12345",
  "name": "Payments API",
  "description": "Handles payment workflows.",
  "criticality": "high",
  "owner": "team-payments",
  "repoUrl": "https://github.com/example/payments",
  "serviceUrl": "https://payments.example.com",
  "language": "java",
  "tags": ["payments", "pci"],
  "createdAt": "2025-10-01T12:34:56Z",
  "updatedAt": "2025-10-01T12:34:56Z"
}
```

---

## Multi-Language Examples

### cURL

```bash
curl -X POST "https://api.appsecplus.example.com/applications" \
  -H "Authorization: Bearer YOUR_ACCESS_TOKEN" \
  -H "Content-Type: application/json" \
  -d '{
    "name": "Payments API",
    "description": "Handles payment workflows.",
    "criticality": "high",
    "owner": "team-payments",
    "repoUrl": "https://github.com/example/payments",
    "serviceUrl": "https://payments.example.com",
    "language": "java",
    "tags": ["payments", "pci"]
  }'
```

### Python (requests)

```python
import requests

url = "https://api.appsecplus.example.com/applications"
headers = {
    "Authorization": "Bearer YOUR_ACCESS_TOKEN",
    "Content-Type": "application/json"
}
payload = {
    "name": "Payments API",
    "description": "Handles payment workflows.",
    "criticality": "high",
    "owner": "team-payments",
    "repoUrl": "https://github.com/example/payments",
    "serviceUrl": "https://payments.example.com",
    "language": "java",
    "tags": ["payments", "pci"]
}

response = requests.post(url, headers=headers, json=payload)
print(response.json())
```

### JavaScript (fetch)

```javascript
const url = "https://api.appsecplus.example.com/applications";

const payload = {
  name: "Payments API",
  description: "Handles payment workflows.",
  criticality: "high",
  owner: "team-payments",
  repoUrl: "https://github.com/example/payments",
  serviceUrl: "https://payments.example.com",
  language: "java",
  tags: ["payments", "pci"]
};

fetch(url, {
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
    + "\"name\":\"Payments API\","
    + "\"description\":\"Handles payment workflows.\","
    + "\"criticality\":\"high\","
    + "\"owner\":\"team-payments\""
    + "}";

RequestBody body = RequestBody.create(JSON, jsonBody);

Request request = new Request.Builder()
    .url("https://api.appsecplus.example.com/applications")
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
        "name": "Payments API",
        "description": "Handles payment workflows.",
        "criticality": "high",
        "owner": "team-payments"
    }`)

    req, _ := http.NewRequest("POST",
        "https://api.appsecplus.example.com/applications",
        bytes.NewBuffer(jsonStr))

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

| HTTP Code | Description                                 |
|----------|---------------------------------------------|
| 400      | Validation error in the request body.       |
| 401      | Authentication failed.                      |
| 403      | Insufficient permissions.                   |
| 409      | Application with the same name already exists. |
| 500      | Internal server error.                      |

---

# GetApplication

Retrieves the details of a single application by ID.

---

## Request Syntax

```http
GET /applications/{id} HTTP/1.1
Host: api.appsecplus.example.com
Authorization: Bearer <access_token>
```

---

## URI Parameters

| Name | Type   | Required | Description                        |
|------|--------|----------|------------------------------------|
| `id` | string | Yes      | The unique ID of the application. |

---

## Response Syntax

```json
{
  "id": "app-12345",
  "name": "Payments API",
  "description": "Handles payment workflows.",
  "criticality": "high",
  "owner": "team-payments",
  "repoUrl": "https://github.com/example/payments",
  "serviceUrl": "https://payments.example.com",
  "language": "java",
  "tags": ["payments", "pci"],
  "createdAt": "2025-10-01T12:34:56Z",
  "updatedAt": "2025-11-01T08:30:00Z"
}
```

---

## Multi-Language Examples

### cURL

```bash
curl -X GET "https://api.appsecplus.example.com/applications/app-12345" \
  -H "Authorization: Bearer YOUR_ACCESS_TOKEN"
```

### Python (requests)

```python
import requests

app_id = "app-12345"
url = f"https://api.appsecplus.example.com/applications/{app_id}"

headers = {
    "Authorization": "Bearer YOUR_ACCESS_TOKEN"
}

response = requests.get(url, headers=headers)
print(response.json())
```

### JavaScript (fetch)

```javascript
const appId = "app-12345";

fetch(`https://api.appsecplus.example.com/applications/${appId}`, {
  method: "GET",
  headers: {
    "Authorization": "Bearer YOUR_ACCESS_TOKEN"
  }
})
  .then(res => res.json())
  .then(data => console.log(data));
```

### Java (OkHttp)

```java
OkHttpClient client = new OkHttpClient();

String appId = "app-12345";

Request request = new Request.Builder()
    .url("https://api.appsecplus.example.com/applications/" + appId)
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
    appID := "app-12345"
    url := "https://api.appsecplus.example.com/applications/" + appID

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

| HTTP Code | Description                |
|----------|----------------------------|
| 401      | Authentication failed.     |
| 403      | Insufficient permissions.  |
| 404      | Application not found.     |
| 500      | Internal server error.     |

---

# UpdateApplication

Updates an existing application resource.

---

## Request Syntax

```http
PUT /applications/{id} HTTP/1.1
Host: api.appsecplus.example.com
Authorization: Bearer <access_token>
Content-Type: application/json

{
  "name": "Payments API",
  "description": "Updated description.",
  "criticality": "critical",
  "owner": "team-payments",
  "tags": ["payments", "pci", "critical"]
}
```

---

## URI Parameters

| Name | Type   | Required | Description                        |
|------|--------|----------|------------------------------------|
| `id` | string | Yes      | The unique ID of the application. |

---

## Request Body

The request body is an `Application` object.  
All fields overwrite the existing values for the application.

---

## Response Syntax

```json
{
  "id": "app-12345",
  "name": "Payments API",
  "description": "Updated description.",
  "criticality": "critical",
  "owner": "team-payments",
  "repoUrl": "https://github.com/example/payments",
  "serviceUrl": "https://payments.example.com",
  "language": "java",
  "tags": ["payments", "pci", "critical"],
  "createdAt": "2025-10-01T12:34:56Z",
  "updatedAt": "2025-11-02T10:15:00Z"
}
```

---

## Multi-Language Examples

### cURL

```bash
curl -X PUT "https://api.appsecplus.example.com/applications/app-12345" \
  -H "Authorization: Bearer YOUR_ACCESS_TOKEN" \
  -H "Content-Type: application/json" \
  -d '{
    "name": "Payments API",
    "description": "Updated description.",
    "criticality": "critical",
    "owner": "team-payments",
    "tags": ["payments", "pci", "critical"]
  }'
```

### Python (requests)

```python
import requests

app_id = "app-12345"
url = f"https://api.appsecplus.example.com/applications/{app_id}"

headers = {
    "Authorization": "Bearer YOUR_ACCESS_TOKEN",
    "Content-Type": "application/json"
}

payload = {
    "name": "Payments API",
    "description": "Updated description.",
    "criticality": "critical",
    "owner": "team-payments",
    "tags": ["payments", "pci", "critical"]
}

response = requests.put(url, headers=headers, json=payload)
print(response.json())
```

### JavaScript (fetch)

```javascript
const appId = "app-12345";
const payload = {
  name: "Payments API",
  description: "Updated description.",
  criticality: "critical",
  owner: "team-payments",
  tags: ["payments", "pci", "critical"]
};

fetch(`https://api.appsecplus.example.com/applications/${appId}`, {
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

String appId = "app-12345";
String jsonBody = "{"
    + "\"name\":\"Payments API\","
    + "\"description\":\"Updated description.\","
    + "\"criticality\":\"critical\""
    + "}";

RequestBody body = RequestBody.create(JSON, jsonBody);

Request request = new Request.Builder()
    .url("https://api.appsecplus.example.com/applications/" + appId)
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
    appID := "app-12345"
    url := "https://api.appsecplus.example.com/applications/" + appID

    jsonStr := []byte(`{
        "name": "Payments API",
        "description": "Updated description.",
        "criticality": "critical",
        "owner": "team-payments"
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

| HTTP Code | Description                         |
|----------|-------------------------------------|
| 400      | Validation error in request body.   |
| 401      | Authentication failed.              |
| 403      | Insufficient permissions.           |
| 404      | Application not found.              |
| 500      | Internal server error.              |

---

# DeleteApplication

Deletes an application resource by ID.

---

## Request Syntax

```http
DELETE /applications/{id} HTTP/1.1
Host: api.appsecplus.example.com
Authorization: Bearer <access_token>
```

---

## URI Parameters

| Name | Type   | Required | Description                        |
|------|--------|----------|------------------------------------|
| `id` | string | Yes      | The unique ID of the application. |

---

## Response Syntax

If successful, this operation returns HTTP status code `204 No Content` and an empty response body.

---

## Multi-Language Examples

### cURL

```bash
curl -X DELETE "https://api.appsecplus.example.com/applications/app-12345" \
  -H "Authorization: Bearer YOUR_ACCESS_TOKEN"
```

### Python (requests)

```python
import requests

app_id = "app-12345"
url = f"https://api.appsecplus.example.com/applications/{app_id}"

headers = {
    "Authorization": "Bearer YOUR_ACCESS_TOKEN"
}

response = requests.delete(url, headers=headers)
print(response.status_code)
```

### JavaScript (fetch)

```javascript
const appId = "app-12345";

fetch(`https://api.appsecplus.example.com/applications/${appId}`, {
  method: "DELETE",
  headers: {
    "Authorization": "Bearer YOUR_ACCESS_TOKEN"
  }
})
  .then(res => console.log(res.status));
```

### Java (OkHttp)

```java
OkHttpClient client = new OkHttpClient();
String appId = "app-12345";

Request request = new Request.Builder()
    .url("https://api.appsecplus.example.com/applications/" + appId)
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
    appID := "app-12345"
    url := "https://api.appsecplus.example.com/applications/" + appID

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

| HTTP Code | Description               |
|----------|---------------------------|
| 401      | Authentication failed.    |
| 403      | Insufficient permissions. |
| 404      | Application not found.    |
| 500      | Internal server error.    |

---

# ListApplicationScans

Lists all scans associated with a specific application.

---

## Request Syntax

```http
GET /applications/{id}/scans HTTP/1.1
Host: api.appsecplus.example.com
Authorization: Bearer <access_token>
```

---

## URI Parameters

| Name | Type   | Required | Description                        |
|------|--------|----------|------------------------------------|
| `id` | string | Yes      | The unique ID of the application. |

---

## Response Syntax

```json
[
  {
    "id": "scan-001",
    "applicationId": "app-12345",
    "scanType": "sast",
    "status": "completed",
    "triggeredBy": "ci-pipeline",
    "startedAt": "2025-11-01T12:00:00Z",
    "completedAt": "2025-11-01T12:05:00Z",
    "summary": {
      "totalFindings": 42,
      "critical": 1,
      "high": 5,
      "medium": 10,
      "low": 26
    }
  }
]
```

---

## Multi-Language Examples

### cURL

```bash
curl -X GET "https://api.appsecplus.example.com/applications/app-12345/scans" \
  -H "Authorization: Bearer YOUR_ACCESS_TOKEN"
```

### Python (requests)

```python
import requests

app_id = "app-12345"
url = f"https://api.appsecplus.example.com/applications/{app_id}/scans"

headers = {
    "Authorization": "Bearer YOUR_ACCESS_TOKEN"
}

response = requests.get(url, headers=headers)
print(response.json())
```

### JavaScript (fetch)

```javascript
const appId = "app-12345";

fetch(`https://api.appsecplus.example.com/applications/${appId}/scans`, {
  method: "GET",
  headers: {
    "Authorization": "Bearer YOUR_ACCESS_TOKEN"
  }
})
  .then(res => res.json())
  .then(data => console.log(data));
```

### Java (OkHttp)

```java
OkHttpClient client = new OkHttpClient();
String appId = "app-12345";

Request request = new Request.Builder()
    .url("https://api.appsecplus.example.com/applications/" + appId + "/scans")
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
    appID := "app-12345"
    url := "https://api.appsecplus.example.com/applications/" + appID + "/scans"

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

| HTTP Code | Description                |
|----------|----------------------------|
| 401      | Authentication failed.     |
| 403      | Insufficient permissions.  |
| 404      | Application not found.     |
| 500      | Internal server error.     |

---

# TriggerApplicationScan

Creates a new scan for the specified application.  
This operation queues the scan and returns immediately.

---

## Request Syntax

```http
POST /applications/{id}/scans HTTP/1.1
Host: api.appsecplus.example.com
Authorization: Bearer <access_token>
Content-Type: application/json

{
  "scanType": "sast",
  "triggeredBy": "ci-pipeline"
}
```

---

## URI Parameters

| Name | Type   | Required | Description                        |
|------|--------|----------|------------------------------------|
| `id` | string | Yes      | The unique ID of the application. |

---

## Request Body

| Field         | Type   | Required | Description |
|---------------|--------|----------|-------------|
| `scanType`    | string | Yes      | The type of scan: `sast`, `dast`, `sca`, or `container`. |
| `triggeredBy` | string | No       | Identifier of the user or system that initiated the scan. |

---

## Response Syntax

```json
{
  "id": "scan-001",
  "applicationId": "app-12345",
  "scanType": "sast",
  "status": "queued",
  "triggeredBy": "ci-pipeline",
  "startedAt": null,
  "completedAt": null,
  "summary": null
}
```

---

## Multi-Language Examples

### cURL

```bash
curl -X POST "https://api.appsecplus.example.com/applications/app-12345/scans" \
  -H "Authorization: Bearer YOUR_ACCESS_TOKEN" \
  -H "Content-Type: application/json" \
  -d '{
    "scanType": "sast",
    "triggeredBy": "ci-pipeline"
  }'
```

### Python (requests)

```python
import requests

app_id = "app-12345"
url = f"https://api.appsecplus.example.com/applications/{app_id}/scans"

headers = {
    "Authorization": "Bearer YOUR_ACCESS_TOKEN",
    "Content-Type": "application/json"
}

payload = {
    "scanType": "sast",
    "triggeredBy": "ci-pipeline"
}

response = requests.post(url, headers=headers, json=payload)
print(response.json())
```

### JavaScript (fetch)

```javascript
const appId = "app-12345";

const payload = {
  scanType: "sast",
  triggeredBy: "ci-pipeline"
};

fetch(`https://api.appsecplus.example.com/applications/${appId}/scans`, {
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

String appId = "app-12345";
String jsonBody = "{"
    + "\"scanType\":\"sast\","
    + "\"triggeredBy\":\"ci-pipeline\""
    + "}";

RequestBody body = RequestBody.create(JSON, jsonBody);

Request request = new Request.Builder()
    .url("https://api.appsecplus.example.com/applications/" + appId + "/scans")
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
    appID := "app-12345"
    url := "https://api.appsecplus.example.com/applications/" + appID + "/scans"

    jsonStr := []byte(`{
        "scanType": "sast",
        "triggeredBy": "ci-pipeline"
    }`)

    req, _ := http.NewRequest("POST", url, bytes.NewBuffer(jsonStr))
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

| HTTP Code | Description                                               |
|----------|-----------------------------------------------------------|
| 400      | Invalid scan type or malformed request body.              |
| 401      | Authentication failed.                                    |
| 403      | Insufficient permissions.                                 |
| 404      | Application not found.                                    |
| 409      | Another conflicting scan is already running for this app. |
| 500      | Internal server error.                                    |

---

# ListApplicationVulnerabilities

Lists vulnerabilities associated with the specified application.  
You can filter by severity and status.

---

## Request Syntax

```http
GET /applications/{id}/vulnerabilities?severity=string&status=string HTTP/1.1
Host: api.appsecplus.example.com
Authorization: Bearer <access_token>
```

---

## URI Parameters

| Name | Type   | Required | Description                        |
|------|--------|----------|------------------------------------|
| `id` | string | Yes      | The unique ID of the application. |

---

## Query Parameters

| Name      | Type   | Required | Description |
|-----------|--------|----------|-------------|
| `severity`| string | No       | Filters vulnerabilities by severity: `low`, `medium`, `high`, or `critical`. |
| `status`  | string | No       | Filters vulnerabilities by status: `open`, `in_progress`, `resolved`, or `accepted`. |

---

## Response Syntax

```json
[
  {
    "id": "vuln-001",
    "applicationId": "app-12345",
    "scanId": "scan-001",
    "title": "SQL Injection",
    "description": "User input is concatenated into SQL statements.",
    "severity": "critical",
    "cwe": "CWE-89",
    "cve": null,
    "cvssScore": 9.8,
    "status": "open",
    "introducedIn": "commit-abc123",
    "remediation": "Use parameterized queries.",
    "createdAt": "2025-11-01T12:05:00Z",
    "updatedAt": "2025-11-01T12:05:00Z"
  }
]
```

---

## Multi-Language Examples

### cURL

```bash
curl -X GET "https://api.appsecplus.example.com/applications/app-12345/vulnerabilities?severity=critical" \
  -H "Authorization: Bearer YOUR_ACCESS_TOKEN"
```

### Python (requests)

```python
import requests

app_id = "app-12345"
url = f"https://api.appsecplus.example.com/applications/{app_id}/vulnerabilities"
params = {"severity": "critical"}

headers = {
    "Authorization": "Bearer YOUR_ACCESS_TOKEN"
}

response = requests.get(url, headers=headers, params=params)
print(response.json())
```

### JavaScript (fetch)

```javascript
const appId = "app-12345";
const url = new URL(`https://api.appsecplus.example.com/applications/${appId}/vulnerabilities`);
url.searchParams.set("severity", "critical");

fetch(url, {
  method: "GET",
  headers: {
    "Authorization": "Bearer YOUR_ACCESS_TOKEN"
  }
})
  .then(res => res.json())
  .then(data => console.log(data));
```

### Java (OkHttp)

```java
OkHttpClient client = new OkHttpClient();
String appId = "app-12345";

HttpUrl url = HttpUrl.parse("https://api.appsecplus.example.com/applications/" + appId + "/vulnerabilities")
    .newBuilder()
    .addQueryParameter("severity", "critical")
    .build();

Request request = new Request.Builder()
    .url(url)
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
    "net/url"
    "io/ioutil"
)

func main() {
    appID := "app-12345"
    base, _ := url.Parse("https://api.appsecplus.example.com/applications/" + appID + "/vulnerabilities")
    params := url.Values{}
    params.Set("severity", "critical")
    base.RawQuery = params.Encode()

    req, _ := http.NewRequest("GET", base.String(), nil)
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

| HTTP Code | Description                |
|----------|----------------------------|
| 400      | Invalid severity or status.|
| 401      | Authentication failed.     |
| 403      | Insufficient permissions.  |
| 404      | Application not found.     |
| 500      | Internal server error.     |

