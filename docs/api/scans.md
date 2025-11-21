# Scans

This section describes the operations you can use to work with scan resources in AppSec+.  
Scans allow you to programmatically trigger static, dynamic, SCA, or container security analysis for your applications.

The scan subsystem is asynchronous. When you trigger a scan, AppSec+ queues the job immediately and returns a Scan object with status `queued`. You can query the scan until it reaches `completed` or `failed`.

---

# ListScans

Lists the scan resources that exist in the AppSec+ environment.  
You can filter the results by application ID or scan status.

---

## Request Syntax

```http
GET /scans?applicationId=string&status=string HTTP/1.1
Host: api.appsecplus.example.com
Authorization: Bearer <access_token>
```

---

## Query Parameters

| Name            | Type   | Required | Description |
|-----------------|--------|----------|-------------|
| `applicationId` | string | No       | Returns scans associated with the specified application. |
| `status`        | string | No       | Returns scans with the specified status. Valid values are `queued`, `running`, `completed`, `failed`. |

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

## Response Elements

| Field            | Type    | Description |
|------------------|---------|-------------|
| `id`             | string  | Unique identifier of the scan. |
| `applicationId`  | string  | ID of the application scanned. |
| `scanType`       | string  | `sast`, `dast`, `sca`, or `container`. |
| `status`         | string  | `queued`, `running`, `completed`, or `failed`. |
| `triggeredBy`    | string  | Identifier of the triggering user/system. |
| `startedAt`      | string  | Timestamp when execution began. |
| `completedAt`    | string  | Timestamp when execution completed. |
| `summary`        | object  | Aggregate findings counts. |

---

## Multi-Language Examples

### cURL

```bash
curl -X GET "https://api.appsecplus.example.com/scans?status=completed" \
  -H "Authorization: Bearer YOUR_ACCESS_TOKEN"
```

### Python (requests)

```python
import requests

url = "https://api.appsecplus.example.com/scans"
params = {"status": "completed"}

headers = {"Authorization": "Bearer YOUR_ACCESS_TOKEN"}

response = requests.get(url, headers=headers, params=params)
print(response.json())
```

### JavaScript (fetch)

```javascript
const url = new URL("https://api.appsecplus.example.com/scans");
url.searchParams.set("status", "completed");

fetch(url, {
  method: "GET",
  headers: { "Authorization": "Bearer YOUR_ACCESS_TOKEN" }
})
  .then(res => res.json())
  .then(data => console.log(data));
```

### Java (OkHttp)

```java
OkHttpClient client = new OkHttpClient();

HttpUrl url = HttpUrl.parse("https://api.appsecplus.example.com/scans")
    .newBuilder()
    .addQueryParameter("status", "completed")
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
    base, _ := url.Parse("https://api.appsecplus.example.com/scans")
    params := url.Values{}
    params.Set("status", "completed")
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

| HTTP Code | Description |
|----------|-------------|
| 401 | Authentication failed. |
| 403 | Insufficient permissions. |
| 500 | Internal server error. |

---

# TriggerScan

Creates a new scan resource.  
This operation queues a scan immediately and returns a `Scan` object with status `queued`.

---

## Request Syntax

```http
POST /scans HTTP/1.1
Host: api.appsecplus.example.com
Authorization: Bearer <access_token>
Content-Type: application/json

{
  "applicationId": "app-12345",
  "scanType": "sast",
  "triggeredBy": "ci-pipeline"
}
```

---

## Request Body

| Field            | Type   | Required | Description |
|------------------|--------|----------|-------------|
| `applicationId`  | string | Yes      | ID of the application to scan. |
| `scanType`       | string | Yes      | Type of scan: `sast`, `dast`, `sca`, or `container`. |
| `triggeredBy`    | string | No       | Identifier of the triggering user/system. |

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
curl -X POST "https://api.appsecplus.example.com/scans" \
  -H "Authorization: Bearer YOUR_ACCESS_TOKEN" \
  -H "Content-Type: application/json" \
  -d '{
    "applicationId": "app-12345",
    "scanType": "sast",
    "triggeredBy": "ci-pipeline"
  }'
```

### Python (requests)

```python
import requests

url = "https://api.appsecplus.example.com/scans"

payload = {
    "applicationId": "app-12345",
    "scanType": "sast",
    "triggeredBy": "ci-pipeline"
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
  applicationId: "app-12345",
  scanType: "sast",
  triggeredBy: "ci-pipeline"
};

fetch("https://api.appsecplus.example.com/scans", {
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
    + "\"applicationId\":\"app-12345\","
    + "\"scanType\":\"sast\","
    + "\"triggeredBy\":\"ci-pipeline\""
    + "}";

RequestBody body = RequestBody.create(JSON, jsonBody);

Request request = new Request.Builder()
    .url("https://api.appsecplus.example.com/scans")
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
        "applicationId": "app-12345",
        "scanType": "sast",
        "triggeredBy": "ci-pipeline"
    }`)

    req, _ := http.NewRequest("POST", "https://api.appsecplus.example.com/scans", bytes.NewBuffer(jsonStr))
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
| 400 | Invalid application ID or scan type. |
| 401 | Authentication failed. |
| 403 | Insufficient permissions. |
| 404 | Application not found. |
| 409 | Conflicting scan is already running. |
| 500 | Internal server error. |

---

# GetScan

Retrieves details for a single scan by ID.

---

## Request Syntax

```http
GET /scans/{id} HTTP/1.1
Host: api.appsecplus.example.com
Authorization: Bearer <access_token>
```

---

## URI Parameters

| Name | Type   | Required | Description |
|------|--------|----------|-------------|
| `id` | string | Yes      | Unique identifier of the scan. |

---

## Response Syntax

```json
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
```

---

## Multi-Language Examples

### cURL

```bash
curl -X GET "https://api.appsecplus.example.com/scans/scan-001" \
  -H "Authorization: Bearer YOUR_ACCESS_TOKEN"
```

### Python (requests)

```python
import requests

scan_id = "scan-001"
url = f"https://api.appsecplus.example.com/scans/{scan_id}"

headers = {"Authorization": "Bearer YOUR_ACCESS_TOKEN"}

response = requests.get(url, headers=headers)
print(response.json())
```

### JavaScript (fetch)

```javascript
fetch("https://api.appsecplus.example.com/scans/scan-001", {
  method: "GET",
  headers: { "Authorization": "Bearer YOUR_ACCESS_TOKEN" }
})
  .then(res => res.json())
  .then(data => console.log(data));
```

### Java (OkHttp)

```java
OkHttpClient client = new OkHttpClient();
String scanId = "scan-001";

Request request = new Request.Builder()
    .url("https://api.appsecplus.example.com/scans/" + scanId)
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
    scanID := "scan-001"
    url := "https://api.appsecplus.example.com/scans/" + scanID

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
| 404 | Scan not found. |
| 500 | Internal server error. |

---

# End of scans.md

