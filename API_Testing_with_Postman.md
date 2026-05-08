# 📮 API Testing with Postman
## Complete Detailed Guide

---

## 📌 Table of Contents

1. [What is API Testing?](#1-what-is-api-testing)
2. [What is Postman?](#2-what-is-postman)
3. [REST API Fundamentals](#3-rest-api-fundamentals)
4. [HTTP Methods & Status Codes](#4-http-methods--status-codes)
5. [Postman Interface Overview](#5-postman-interface-overview)
6. [Creating API Requests](#6-creating-api-requests)
7. [Request Headers & Authentication](#7-request-headers--authentication)
8. [Request Body Types](#8-request-body-types)
9. [Writing Tests in Postman (JavaScript)](#9-writing-tests-in-postman-javascript)
10. [Environment & Global Variables](#10-environment--global-variables)
11. [Collections & Collection Runner](#11-collections--collection-runner)
12. [Data-Driven Testing](#12-data-driven-testing)
13. [Pre-request Scripts](#13-pre-request-scripts)
14. [Postman Mock Servers](#14-postman-mock-servers)
15. [Newman — CLI for Postman](#15-newman--cli-for-postman)
16. [API Testing in CI/CD (Jenkins + Newman)](#16-api-testing-in-cicd-jenkins--newman)
17. [GraphQL Testing in Postman](#17-graphql-testing-in-postman)
18. [Best Practices](#18-best-practices)
19. [Common Interview Questions](#19-common-interview-questions)

---

## 1. What is API Testing?

**API Testing** is a type of software testing that validates APIs — checking that they function correctly, reliably, and securely. Unlike UI testing, API testing operates at the **business logic layer**.

### Types of API Tests

| Test Type | Purpose |
|---|---|
| **Functional Testing** | Does the API return the correct response? |
| **Validation Testing** | Are the response fields/types/values correct? |
| **Security Testing** | Is authentication enforced? Injection safe? |
| **Performance Testing** | How fast does the API respond under load? |
| **Negative Testing** | Does the API handle bad inputs gracefully? |
| **Integration Testing** | Do multiple APIs work together correctly? |
| **Regression Testing** | Do existing APIs still work after changes? |
| **Contract Testing** | Does the API response match the agreed schema? |

### Why API Testing?

- ✅ Faster than UI testing (no browser rendering)
- ✅ Catches bugs earlier in development cycle
- ✅ Works without a UI (backend-only testing)
- ✅ Language and platform independent
- ✅ Essential for microservices architecture

---

## 2. What is Postman?

**Postman** is a popular API platform for building, testing, and documenting APIs. Originally built as a Chrome extension in 2012, it's now a full-featured desktop application.

| Feature | Details |
|---|---|
| **Type** | API testing & development platform |
| **Language** | JavaScript (for test scripts) |
| **Platforms** | Windows, macOS, Linux, Web |
| **License** | Free (Basic) + Paid (Team/Enterprise) |
| **CLI Tool** | Newman (run Postman collections from terminal) |
| **Website** | [postman.com](https://www.postman.com) |

### Postman Capabilities

- 🔹 Send HTTP/HTTPS requests (GET, POST, PUT, DELETE, PATCH)
- 🔹 Write automated tests using JavaScript
- 🔹 Organize requests in Collections
- 🔹 Use Environments for different configs (dev/staging/prod)
- 🔹 Data-driven testing using CSV/JSON files
- 🔹 Mock servers for API simulation
- 🔹 API documentation generation
- 🔹 Newman for CI/CD integration
- 🔹 Monitors for scheduled API health checks
- 🔹 GraphQL support

---

## 3. REST API Fundamentals

### What is REST?

**REST** (Representational State Transfer) is an architectural style for designing networked applications. REST APIs use HTTP and are stateless.

### REST Principles

| Principle | Description |
|---|---|
| **Stateless** | Each request is independent; server stores no client state |
| **Client-Server** | Separation between UI (client) and data storage (server) |
| **Uniform Interface** | Consistent URL structure and HTTP methods |
| **Cacheable** | Responses can be cached for performance |
| **Layered System** | Client doesn't know about server internals |

### API Request Anatomy

```
Method   URL                          Headers      Body
  │       │                              │            │
POST  https://api.example.com/users   Content-Type  { "name": "John" }
               │           │
             Base URL    Endpoint
```

### URL Structure

```
https://api.example.com/v1/users/123?include=profile&format=json
│      │               │  │     │   │
│      │               │  │     │   └─ Query Params
│      │               │  │     └───── Path Param (user ID)
│      │               │  └─────────── Resource
│      │               └────────────── API Version
│      └────────────────────────────── Domain
└───────────────────────────────────── Protocol
```

---

## 4. HTTP Methods & Status Codes

### HTTP Methods (CRUD Operations)

| HTTP Method | CRUD | Purpose | Has Body? |
|---|---|---|---|
| **GET** | Read | Retrieve resource(s) | ❌ No |
| **POST** | Create | Create new resource | ✅ Yes |
| **PUT** | Update | Replace entire resource | ✅ Yes |
| **PATCH** | Update | Partially update resource | ✅ Yes |
| **DELETE** | Delete | Remove resource | ❌ Usually No |
| **OPTIONS** | — | Get allowed methods | ❌ No |
| **HEAD** | — | GET without body | ❌ No |

### HTTP Status Codes

#### 2xx — Success

| Code | Name | Meaning |
|---|---|---|
| **200** | OK | Request successful |
| **201** | Created | Resource successfully created |
| **202** | Accepted | Request accepted, processing async |
| **204** | No Content | Success but no body (DELETE) |

#### 3xx — Redirection

| Code | Name | Meaning |
|---|---|---|
| **301** | Moved Permanently | Resource permanently moved |
| **302** | Found | Temporary redirect |
| **304** | Not Modified | Cached version is valid |

#### 4xx — Client Errors

| Code | Name | Meaning |
|---|---|---|
| **400** | Bad Request | Invalid syntax or request |
| **401** | Unauthorized | Authentication required |
| **403** | Forbidden | Authenticated but no permission |
| **404** | Not Found | Resource doesn't exist |
| **405** | Method Not Allowed | HTTP method not supported |
| **409** | Conflict | Resource conflict (duplicate) |
| **422** | Unprocessable Entity | Validation errors |
| **429** | Too Many Requests | Rate limit exceeded |

#### 5xx — Server Errors

| Code | Name | Meaning |
|---|---|---|
| **500** | Internal Server Error | Generic server error |
| **502** | Bad Gateway | Invalid upstream response |
| **503** | Service Unavailable | Server is down/overloaded |
| **504** | Gateway Timeout | Upstream server timed out |

---

## 5. Postman Interface Overview

```
┌─────────────────────────────────────────────────────────────────┐
│  POSTMAN APPLICATION                                            │
│                                                                 │
│  ┌──────────────┐  ┌──────────────────────────────────────┐   │
│  │  Sidebar     │  │  Request Builder                      │   │
│  │              │  │                                        │   │
│  │  📁 My       │  │  [GET ▼] [URL Input Field    ] [Send] │   │
│  │  Collections │  │                                        │   │
│  │              │  │  Params │ Auth │ Headers │ Body │ Pre  │   │
│  │  🌍 Envs     │  │                                        │   │
│  │              │  ├────────────────────────────────────────┤   │
│  │  📜 History  │  │  Response                              │   │
│  │              │  │                                        │   │
│  │  🔗 APIs     │  │  Status: 200 OK | Time: 245ms | 1.2KB  │   │
│  │              │  │  Body │ Cookies │ Headers │ Test Results│  │
│  └──────────────┘  └──────────────────────────────────────┘   │
└─────────────────────────────────────────────────────────────────┘
```

### Key Sections

| Section | Description |
|---|---|
| **Collections** | Groups of saved API requests |
| **Environments** | Variable sets (dev, staging, prod) |
| **History** | Previously sent requests |
| **Request Builder** | Where you compose API calls |
| **Response Panel** | Shows API response |
| **Test Results** | Shows pass/fail of test scripts |
| **Console** | Logs from scripts |

---

## 6. Creating API Requests

### GET Request — Fetch a User

```
Method:  GET
URL:     https://reqres.in/api/users/2
Headers: (none required)
Body:    (none)
```

**Expected Response (200 OK):**
```json
{
  "data": {
    "id": 2,
    "email": "janet.weaver@reqres.in",
    "first_name": "Janet",
    "last_name": "Weaver",
    "avatar": "https://reqres.in/img/faces/2-image.jpg"
  },
  "support": {
    "url": "https://reqres.in/#support-heading",
    "text": "To keep ReqRes free..."
  }
}
```

### GET with Query Parameters

```
Method:  GET
URL:     https://reqres.in/api/users
Params:
  Key: page    Value: 2
  Key: per_page  Value: 6
```

This builds URL: `https://reqres.in/api/users?page=2&per_page=6`

### POST Request — Create a User

```
Method:  POST
URL:     https://reqres.in/api/users
Headers:
  Content-Type: application/json
Body (raw JSON):
  {
    "name": "John Doe",
    "job": "Software Engineer"
  }
```

**Expected Response (201 Created):**
```json
{
  "name": "John Doe",
  "job": "Software Engineer",
  "id": "867",
  "createdAt": "2024-01-15T10:30:00.000Z"
}
```

### PUT Request — Update a User

```
Method:  PUT
URL:     https://reqres.in/api/users/2
Headers:
  Content-Type: application/json
Body:
  {
    "name": "Janet Updated",
    "job": "Senior Developer"
  }
```

### PATCH Request — Partial Update

```
Method:  PATCH
URL:     https://reqres.in/api/users/2
Body:
  {
    "job": "Team Lead"
  }
```

### DELETE Request

```
Method:  DELETE
URL:     https://reqres.in/api/users/2
Expected: 204 No Content
```

---

## 7. Request Headers & Authentication

### Common Headers

```
Content-Type: application/json        ← Format of request body
Accept: application/json              ← Format expected in response
Authorization: Bearer <token>         ← JWT auth token
X-API-Key: your-api-key              ← API key authentication
Accept-Language: en-US                ← Language preference
Cache-Control: no-cache               ← Caching behavior
```

### Authentication Types in Postman

#### 1. No Auth
For public APIs that don't require authentication.

#### 2. API Key

```
Key:  X-API-Key
Value: abc123xyz
Add to: Header (or Query Params)
```

#### 3. Bearer Token (JWT)

```
Auth Type: Bearer Token
Token: eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9...
```

**Auto-added Header:**
```
Authorization: Bearer eyJhbGciOiJIUzI1NiIs...
```

#### 4. Basic Auth

```
Auth Type: Basic Auth
Username: admin
Password: password123
```

**Auto-added Header:**
```
Authorization: Basic YWRtaW46cGFzc3dvcmQxMjM=
```

#### 5. OAuth 2.0

```
Auth Type: OAuth 2.0
Grant Type: Authorization Code / Client Credentials
Access Token URL: https://auth.example.com/oauth/token
Client ID: your-client-id
Client Secret: your-secret
Scope: read write
```

#### 6. Digest Auth

```
Auth Type: Digest Auth
Username: user
Password: pass
```

---

## 8. Request Body Types

| Body Type | Use Case | Content-Type |
|---|---|---|
| **none** | GET, DELETE | — |
| **form-data** | File uploads, form submissions | `multipart/form-data` |
| **x-www-form-urlencoded** | HTML form data | `application/x-www-form-urlencoded` |
| **raw (JSON)** | REST API calls | `application/json` |
| **raw (XML)** | SOAP/XML APIs | `application/xml` |
| **raw (Text)** | Plain text payloads | `text/plain` |
| **binary** | File upload (image/PDF) | — |
| **GraphQL** | GraphQL queries | `application/json` |

### JSON Body Example

```json
{
  "user": {
    "firstName": "John",
    "lastName": "Doe",
    "email": "john@example.com",
    "age": 30,
    "address": {
      "street": "123 Main St",
      "city": "New York",
      "zipCode": "10001"
    },
    "roles": ["admin", "editor"],
    "isActive": true
  }
}
```

### Form-Data Example (with File Upload)

```
Key: name      Value: John Doe     Type: Text
Key: email     Value: j@e.com      Type: Text
Key: avatar    Value: [file]       Type: File
```

---

## 9. Writing Tests in Postman (JavaScript)

Postman uses **JavaScript (Chai.js assertions)** in the **Tests** tab to write automated test scripts.

### Basic Test Structure

```javascript
pm.test("Test Description", function() {
    // Assertion here
    pm.expect(actual).to.equal(expected);
});
```

### ─── Status Code Tests ───────────────────────────────────────────

```javascript
// Check status code is 200
pm.test("Status code is 200", function() {
    pm.response.to.have.status(200);
});

// Check status code is in range 200-299
pm.test("Status is success", function() {
    pm.expect(pm.response.code).to.be.within(200, 299);
});

// Check status message
pm.test("Status message is OK", function() {
    pm.response.to.have.status("OK");
});
```

### ─── Response Body Tests ─────────────────────────────────────────

```javascript
// Parse JSON response
const jsonData = pm.response.json();

// Check specific field value
pm.test("User email is correct", function() {
    pm.expect(jsonData.data.email).to.equal("janet.weaver@reqres.in");
});

// Check field exists
pm.test("Response has 'id' field", function() {
    pm.expect(jsonData.data).to.have.property("id");
});

// Check field is not null/undefined
pm.test("ID is not null", function() {
    pm.expect(jsonData.data.id).to.not.be.null;
    pm.expect(jsonData.data.id).to.not.be.undefined;
});

// Check data type
pm.test("ID is a number", function() {
    pm.expect(jsonData.data.id).to.be.a("number");
});

pm.test("Email is a string", function() {
    pm.expect(jsonData.data.email).to.be.a("string");
});

// Check array length
pm.test("User list has 6 items", function() {
    pm.expect(jsonData.data).to.have.lengthOf(6);
});

// Check array is not empty
pm.test("Data array is not empty", function() {
    pm.expect(jsonData.data).to.not.be.empty;
});

// Check value is within range
pm.test("User ID is between 1 and 100", function() {
    pm.expect(jsonData.data.id).to.be.within(1, 100);
});

// Check string contains
pm.test("Email contains '@'", function() {
    pm.expect(jsonData.data.email).to.include("@");
});

// Check boolean
pm.test("User is active", function() {
    pm.expect(jsonData.isActive).to.be.true;
});
```

### ─── Response Header Tests ──────────────────────────────────────

```javascript
// Check header exists
pm.test("Content-Type header is present", function() {
    pm.response.to.have.header("Content-Type");
});

// Check header value
pm.test("Content-Type is JSON", function() {
    pm.expect(pm.response.headers.get("Content-Type"))
      .to.include("application/json");
});
```

### ─── Response Time Tests ─────────────────────────────────────────

```javascript
// Check response time
pm.test("Response time is under 2 seconds", function() {
    pm.expect(pm.response.responseTime).to.be.below(2000);
});
```

### ─── Negative Tests ──────────────────────────────────────────────

```javascript
// Check 404 for non-existent resource
pm.test("User not found returns 404", function() {
    pm.response.to.have.status(404);
});

// Check error message in body
pm.test("Error message is returned", function() {
    const jsonData = pm.response.json();
    pm.expect(jsonData).to.have.property("error");
    pm.expect(jsonData.error).to.equal("user not found");
});
```

### ─── Schema Validation ───────────────────────────────────────────

```javascript
// Validate response matches JSON Schema
const schema = {
    type: "object",
    properties: {
        data: {
            type: "object",
            properties: {
                id:         { type: "number" },
                email:      { type: "string" },
                first_name: { type: "string" },
                last_name:  { type: "string" },
                avatar:     { type: "string" }
            },
            required: ["id", "email", "first_name", "last_name"]
        }
    },
    required: ["data"]
};

pm.test("Response matches JSON schema", function() {
    pm.response.to.have.jsonSchema(schema);
});
```

### ─── Chaining Requests (Setting Variables) ──────────────────────

```javascript
// Extract and store token from login response
const jsonData = pm.response.json();
pm.environment.set("authToken", jsonData.token);
pm.environment.set("userId", jsonData.user.id);
console.log("Token saved:", jsonData.token);
```

---

## 10. Environment & Global Variables

### Variable Scopes (Priority Order)

```
Global → Collection → Environment → Data → Local
  (lowest priority)                 (highest priority)
```

### Variable Types

| Scope | Set Via | Access Via | Use Case |
|---|---|---|---|
| **Global** | Scripts / UI | `pm.globals` | Shared across all collections |
| **Collection** | Collection → Variables | `pm.collectionVariables` | Shared within a collection |
| **Environment** | Environment editor | `pm.environment` | Per environment (dev/staging/prod) |
| **Data** | CSV/JSON file | `pm.iterationData` | Data-driven testing |
| **Local** | Scripts only | `pm.variables` | Temporary, request-scoped |

### Setting Variables in Scripts

```javascript
// Environment variables
pm.environment.set("token", "abc123");
pm.environment.set("userId", 42);

// Global variables
pm.globals.set("baseUrl", "https://api.example.com");

// Collection variables
pm.collectionVariables.set("version", "v1");

// Get variable
const token = pm.environment.get("token");
const baseUrl = pm.globals.get("baseUrl");

// Unset variable
pm.environment.unset("tempVar");

// Check variable exists
const hasToken = pm.environment.has("token");
```

### Using Variables in Requests

```
URL:     {{baseUrl}}/{{version}}/users
Headers: Authorization: Bearer {{authToken}}
Body:    {"userId": "{{userId}}"}
```

### Sample Environment Setup

**Development Environment:**
```
baseUrl    = https://api-dev.example.com
apiKey     = dev-key-abc123
authToken  = (set dynamically via scripts)
userId     = (set dynamically via scripts)
```

**Staging Environment:**
```
baseUrl    = https://api-staging.example.com
apiKey     = staging-key-xyz789
authToken  = (set dynamically via scripts)
```

---

## 11. Collections & Collection Runner

### What is a Collection?

A **Collection** is a group of organized API requests, usually representing a complete workflow or a feature's API set.

### Collection Structure Example

```
📁 User Management API
│
├── 📂 Auth
│   ├── POST /login         ← Gets auth token
│   └── POST /logout        ← Clears token
│
├── 📂 Users
│   ├── GET  /users         ← List all users
│   ├── POST /users         ← Create user
│   ├── GET  /users/:id     ← Get single user
│   ├── PUT  /users/:id     ← Update user
│   └── DELETE /users/:id  ← Delete user
│
└── 📂 Admin
    ├── GET  /admin/stats   ← Dashboard stats
    └── GET  /admin/logs    ← Activity logs
```

### Collection Runner

The **Collection Runner** runs all requests in a collection sequentially. Access via:
- **Postman UI:** Runner → Select Collection → Run

**Options:**
- Set number of **iterations**
- Add a **data file** (CSV/JSON)
- Set **delay** between requests
- Choose specific **environment**
- Select specific requests to run

---

## 12. Data-Driven Testing

### Using CSV File

**`users_data.csv`:**
```csv
username,password,expectedStatus
admin@example.com,Admin@123,200
user@example.com,User@123,200
wrong@example.com,wrongpass,401
,Admin@123,400
```

**Pre-request Script:**
```javascript
const username = pm.iterationData.get("username");
const password = pm.iterationData.get("password");

pm.environment.set("username", username);
pm.environment.set("password", password);
```

**Request Body:**
```json
{
    "username": "{{username}}",
    "password": "{{password}}"
}
```

**Tests:**
```javascript
const expectedStatus = parseInt(pm.iterationData.get("expectedStatus"));

pm.test("Status code matches expected: " + expectedStatus, function() {
    pm.response.to.have.status(expectedStatus);
});
```

### Using JSON File

**`test_data.json`:**
```json
[
  {
    "name": "Alice Johnson",
    "email": "alice@example.com",
    "role": "admin",
    "expectedId": 1
  },
  {
    "name": "Bob Smith",
    "email": "bob@example.com",
    "role": "user",
    "expectedId": 2
  }
]
```

**Test Script:**
```javascript
const name = pm.iterationData.get("name");
const email = pm.iterationData.get("email");

pm.test("Created user name matches: " + name, function() {
    const jsonData = pm.response.json();
    pm.expect(jsonData.name).to.equal(name);
    pm.expect(jsonData.email).to.equal(email);
});
```

---

## 13. Pre-request Scripts

Pre-request scripts run **before** the request is sent. Use them to:
- Set dynamic variables
- Generate timestamps, random data
- Calculate auth signatures
- Set up dependencies

### Generating Random Test Data

```javascript
// Generate random user data
const timestamp = Date.now();
const randomId = Math.floor(Math.random() * 1000);

pm.environment.set("uniqueEmail", `testuser${timestamp}@example.com`);
pm.environment.set("randomUserId", randomId);
pm.environment.set("timestamp", timestamp);

// Generate random name
const firstNames = ["Alice", "Bob", "Charlie", "Diana", "Eve"];
const randomName = firstNames[Math.floor(Math.random() * firstNames.length)];
pm.environment.set("randomName", randomName);

console.log("Generated email:", `testuser${timestamp}@example.com`);
```

### Auto-Login (Get Fresh Token Before Each Request)

```javascript
// Pre-request: Get token if it doesn't exist or is expired
const token = pm.environment.get("authToken");
const tokenExpiry = pm.environment.get("tokenExpiry");
const now = Date.now();

if (!token || now > parseInt(tokenExpiry)) {
    pm.sendRequest({
        url: pm.environment.get("baseUrl") + "/auth/login",
        method: "POST",
        header: { "Content-Type": "application/json" },
        body: {
            mode: "raw",
            raw: JSON.stringify({
                username: "admin@example.com",
                password: "Admin@123"
            })
        }
    }, function(err, response) {
        if (!err) {
            const data = response.json();
            pm.environment.set("authToken", data.token);
            // Set expiry to 1 hour from now
            pm.environment.set("tokenExpiry", now + 3600000);
            console.log("Token refreshed successfully");
        }
    });
}
```

### HMAC Signature Generation

```javascript
// Generate HMAC-SHA256 signature for API authentication
const CryptoJS = require("crypto-js");
const secretKey = pm.environment.get("secretKey");
const timestamp = Date.now().toString();
const method = pm.request.method;
const path = pm.request.url.getPath();

const message = timestamp + method + path;
const signature = CryptoJS.HmacSHA256(message, secretKey).toString();

pm.environment.set("timestamp", timestamp);
pm.environment.set("signature", signature);
```

---

## 14. Postman Mock Servers

**Mock Servers** let you simulate API responses without a real backend.

### Creating a Mock Server

1. Open a **Collection** → Click **...** → **Mock Collection**
2. Name the mock server
3. Copy the **Mock URL**

### Defining Mock Responses (Examples)

For each request, add **Examples** with:
- Request method + URL
- Response status code
- Response headers
- Response body (JSON)

**Example — GET /users:**
```json
// Status: 200 OK
{
  "users": [
    { "id": 1, "name": "Alice", "email": "alice@example.com" },
    { "id": 2, "name": "Bob", "email": "bob@example.com" }
  ],
  "total": 2
}
```

**Example — POST /users (400):**
```json
// Status: 400 Bad Request
{
  "error": "Validation failed",
  "details": {
    "email": "Email is required"
  }
}
```

### Using Mock in Tests

```javascript
// Point requests to mock URL
pm.environment.set("baseUrl", "https://mock.example.postman.com");
```

---

## 15. Newman — CLI for Postman

**Newman** is the command-line companion to Postman, allowing you to run collections from the terminal — perfect for CI/CD pipelines.

### Installation

```bash
# Install Newman globally
npm install -g newman

# Install HTML Extra reporter (better reports)
npm install -g newman-reporter-htmlextra
```

### Basic Commands

```bash
# Run collection from exported JSON file
newman run MyCollection.json

# Run with environment file
newman run MyCollection.json -e MyEnvironment.json

# Run with data file (CSV)
newman run MyCollection.json -d test_data.csv

# Run with data file (JSON)
newman run MyCollection.json -d test_data.json

# Set number of iterations
newman run MyCollection.json -n 5

# Set timeout (ms)
newman run MyCollection.json --timeout-request 5000

# Run specific folder only
newman run MyCollection.json --folder "User Tests"
```

### Generate Reports

```bash
# HTML Extra report (recommended)
newman run MyCollection.json \
  -e environment.json \
  --reporters htmlextra \
  --reporter-htmlextra-export reports/newman-report.html \
  --reporter-htmlextra-title "API Test Report"

# Multiple reporters
newman run MyCollection.json \
  -e environment.json \
  --reporters cli,htmlextra,junit \
  --reporter-junit-export reports/results.xml \
  --reporter-htmlextra-export reports/report.html

# JSON reporter
newman run MyCollection.json \
  --reporters json \
  --reporter-json-export reports/results.json
```

### Newman Exit Codes

| Code | Meaning |
|---|---|
| `0` | All tests passed |
| `1` | One or more tests failed |
| `2` | Collection or environment not found |
| `3` | Newman error |

---

## 16. API Testing in CI/CD (Jenkins + Newman)

### Jenkins Pipeline with Newman

```groovy
pipeline {
    agent any

    environment {
        POSTMAN_COLLECTION = "src/test/postman/UserAPI.collection.json"
        POSTMAN_ENV        = "src/test/postman/QA-Environment.json"
        REPORT_DIR         = "reports/newman"
    }

    stages {

        stage('Install Newman') {
            steps {
                sh '''
                    npm install -g newman
                    npm install -g newman-reporter-htmlextra
                '''
            }
        }

        stage('Run API Tests') {
            steps {
                sh """
                    mkdir -p ${REPORT_DIR}
                    newman run ${POSTMAN_COLLECTION} \
                      --environment ${POSTMAN_ENV} \
                      --reporters cli,htmlextra,junit \
                      --reporter-htmlextra-export ${REPORT_DIR}/report.html \
                      --reporter-htmlextra-title "API Test Report - Build #${BUILD_NUMBER}" \
                      --reporter-junit-export ${REPORT_DIR}/results.xml \
                      --timeout-request 10000 \
                      --bail
                """
            }
        }
    }

    post {
        always {
            // Publish JUnit results
            junit "${REPORT_DIR}/results.xml"

            // Publish HTML report
            publishHTML(target: [
                allowMissing: false,
                alwaysLinkToLastBuild: true,
                keepAll: true,
                reportDir: "${REPORT_DIR}",
                reportFiles: "report.html",
                reportName: "Newman API Test Report"
            ])
        }
        success {
            echo "✅ All API tests passed!"
        }
        failure {
            echo "❌ API tests failed!"
            mail to: 'team@example.com',
                 subject: "API Tests FAILED: ${JOB_NAME} #${BUILD_NUMBER}",
                 body: "Check the report: ${BUILD_URL}"
        }
    }
}
```

### `package.json` for Newman

```json
{
  "name": "api-tests",
  "version": "1.0.0",
  "description": "Postman API Tests",
  "scripts": {
    "test": "newman run collections/API.json -e environments/qa.json --reporters htmlextra --reporter-htmlextra-export reports/report.html",
    "test:smoke": "newman run collections/API.json -e environments/qa.json --folder Smoke",
    "test:regression": "newman run collections/API.json -e environments/qa.json -n 3"
  },
  "devDependencies": {
    "newman": "^6.0.0",
    "newman-reporter-htmlextra": "^1.22.0"
  }
}
```

---

## 17. GraphQL Testing in Postman

### Setting Up GraphQL Request

```
Method:  POST
URL:     https://api.example.com/graphql
Body type: GraphQL
```

### GraphQL Query

```graphql
query GetUser($id: ID!) {
  user(id: $id) {
    id
    name
    email
    posts {
      title
      createdAt
    }
  }
}
```

### GraphQL Variables

```json
{
  "id": "123"
}
```

### GraphQL Mutation

```graphql
mutation CreateUser($input: CreateUserInput!) {
  createUser(input: $input) {
    id
    name
    email
    createdAt
  }
}
```

### GraphQL Tests

```javascript
const jsonData = pm.response.json();

pm.test("No GraphQL errors", function() {
    pm.expect(jsonData.errors).to.be.undefined;
});

pm.test("User data is returned", function() {
    pm.expect(jsonData.data.user).to.not.be.null;
    pm.expect(jsonData.data.user.id).to.equal("123");
});
```

---

## 18. Best Practices

### Request Design
- ✅ Use **environment variables** for base URLs and tokens — never hardcode
- ✅ Name requests descriptively: `POST - Create User` not just `Create`
- ✅ Add **descriptions** to requests and collections (they become documentation)
- ✅ Organize requests in **folders** (by feature or by CRUD operation)
- ✅ Always set `Content-Type` header for POST/PUT/PATCH requests

### Test Script Best Practices
- ✅ Test **status code** in every request
- ✅ Test **response time** (use < 2000ms as a baseline)
- ✅ Test **response schema** for critical endpoints
- ✅ Test **specific field values**, not just that they exist
- ✅ Include **negative tests** (invalid data, missing auth, wrong method)
- ✅ Use `pm.test()` for all assertions — not raw `console.log()`
- ✅ Chain requests: extract tokens/IDs in Tests → use in next request

### Collection Management
- ✅ Store collections in **version control (Git)**
- ✅ Use **Collection Variables** for values shared across requests
- ✅ Add **Pre-request Scripts** at collection level for global setup
- ✅ Add **Tests** at collection level for global assertions
- ✅ Export collection as `v2.1` format for Newman compatibility

### CI/CD Integration
- ✅ Run collections via **Newman** in CI pipelines
- ✅ Generate **HTML + JUnit** reports for Jenkins publishing
- ✅ Use **`--bail`** flag to stop Newman on first failure
- ✅ Set `--timeout-request` to prevent hanging
- ✅ Use separate **environment files** per deployment stage

---

## 19. Common Interview Questions

**Q1. What is the difference between GET and POST?**
> GET retrieves data and has no body; it should be idempotent. POST sends data in the body to create a resource and is not idempotent.

**Q2. What is the difference between PUT and PATCH?**
> PUT replaces the entire resource. PATCH partially updates specific fields only.

**Q3. What does a 401 vs 403 status code mean?**
> 401 (Unauthorized) means the client is not authenticated. 403 (Forbidden) means the client is authenticated but lacks permission.

**Q4. How do you handle authentication in Postman?**
> By using the Auth tab (Bearer Token, Basic Auth, OAuth 2.0) or by fetching the token in a Pre-request script and storing it in an environment variable.

**Q5. What is the difference between Environment and Global variables?**
> Environment variables are scoped to a specific environment (dev/staging/prod). Global variables are accessible across all collections and environments.

**Q6. What is Newman and why is it used?**
> Newman is the CLI tool for Postman that allows running collections from the command line, enabling API test automation in CI/CD pipelines without the Postman GUI.

**Q7. How do you chain requests in Postman?**
> By extracting values (tokens, IDs) from a response in the Tests tab using `pm.environment.set()` and using them in subsequent requests as `{{variableName}}`.

**Q8. What is JSON Schema validation in Postman?**
> Using `pm.response.to.have.jsonSchema(schema)` to validate that the API response structure matches a defined JSON Schema, ensuring the API contract is maintained.

**Q9. How do you do data-driven testing in Postman?**
> By using the Collection Runner with a CSV or JSON data file. Each row/object in the file becomes one iteration, and values are accessed via `pm.iterationData.get("key")`.

**Q10. What is a Mock Server in Postman?**
> A Postman Mock Server simulates an API by returning predefined responses without a real backend. It's useful for frontend development or testing before the API is ready.

**Q11. How do you test APIs that require a fresh token for each request?**
> By writing a Pre-request Script that checks if the token is expired, then makes a login request using `pm.sendRequest()` to fetch a new token and save it to the environment.

**Q12. What is the difference between `pm.test()` and `pm.expect()`?**
> `pm.test()` defines a named test case that appears in the Test Results panel. `pm.expect()` is the assertion inside a test. You can have multiple `pm.expect()` calls inside one `pm.test()`.

---

## ▶ Quick Reference Card

```javascript
// ─── Response assertions ───────────────────────────────────────
pm.response.to.have.status(200);
pm.response.to.have.header("Content-Type");
pm.expect(pm.response.responseTime).to.be.below(2000);

// ─── JSON parsing ─────────────────────────────────────────────
const data = pm.response.json();

// ─── Field assertions ─────────────────────────────────────────
pm.expect(data.name).to.equal("John");
pm.expect(data.id).to.be.a("number");
pm.expect(data.items).to.have.lengthOf(3);
pm.expect(data.email).to.include("@");
pm.expect(data.isActive).to.be.true;
pm.expect(data.price).to.be.within(10, 100);
pm.expect(data).to.have.property("id");
pm.expect(data.error).to.be.undefined;

// ─── Variable management ──────────────────────────────────────
pm.environment.set("key", "value");
pm.environment.get("key");
pm.environment.unset("key");
pm.globals.set("key", "value");
pm.collectionVariables.set("key", "value");

// ─── Console logging ──────────────────────────────────────────
console.log("Response:", JSON.stringify(data, null, 2));
```

---

*Official Documentation:*
- *Postman: https://learning.postman.com/docs*
- *Newman: https://www.npmjs.com/package/newman*
- *Chai.js Assertions: https://www.chaijs.com/api/bdd*
