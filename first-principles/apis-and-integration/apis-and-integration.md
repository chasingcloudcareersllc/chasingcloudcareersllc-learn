---
title: "APIs and Integration"
description: "REST, authentication, data formats, client resilience, GraphQL, gRPC, and webhooks."
position: 10
icon: "plug"
---

# APIs and Integration

Modern software systems are not monoliths. They are collections of components that communicate over well-defined boundaries. The boundary between two software components is an Application Programming Interface — an API. Every time you run a shell command, call a library function, or send an HTTP request to a remote service, you are using an API. This domain teaches you how APIs work at the protocol level, how to consume them reliably, how to authenticate to them securely, and how to design them well.

This domain builds directly on [Programming Fundamentals](/learn/first-principles/programming-fundamentals/) (Domain 5) and [Networking](/learn/first-principles/networking/) (Domain 7). You need Python for making API calls programmatically. You need HTTP, DNS, and TLS to understand what happens on the wire when those calls execute.

---

## Why It Matters

Every infrastructure role interacts with APIs daily. Terraform calls cloud provider APIs to create resources. Kubernetes exposes an API server that `kubectl` communicates with over HTTPS. CI/CD pipelines authenticate to GitHub, Docker Hub, and artifact registries through API tokens. Monitoring systems push and pull metrics through APIs. Configuration management tools query inventory APIs. Even `apt install` and `yum update` fetch package metadata from HTTP endpoints.

If you cannot read API documentation, authenticate correctly, handle errors gracefully, and reason about rate limits and retries, you will be blocked constantly. If you can, you can integrate any system with any other system — because the patterns are universal.

---

## What You'll Learn

- What an API is at every level: library, operating system, and web
- REST constraints and how to evaluate whether an API is truly RESTful
- JSON and YAML syntax, parsing with `jq` and Python, and common pitfalls
- Making API calls with `curl` and Python `requests`, including authentication and error handling
- Authentication mechanisms: API keys, Basic auth, Bearer/JWT, and OAuth 2.0 flows
- API design principles: versioning, pagination, rate limiting, idempotency
- Client resilience patterns: retries, exponential backoff, circuit breakers, timeouts
- Alternative API paradigms: GraphQL, gRPC, WebSockets, webhooks, and SSE
- Reading OpenAPI specifications and using tools like Swagger UI and Postman

---

## A. What an API Is

### Theory

An API is a contract between two software components. It defines what you can ask for, how you ask for it, and what you get back. The contract specifies inputs, outputs, error conditions, and guarantees. APIs exist at every level of the computing stack.

**Library APIs** define functions and classes you call within the same process. When you write `import json; json.loads(data)` in Python, you are using the `json` module's API. The contract is the function signature: `loads()` takes a string, returns a Python object, and raises `json.JSONDecodeError` if the string is not valid JSON.

**Operating system APIs (syscalls)** define the boundary between user space and kernel space. When a program opens a file, it invokes the `open()` system call. The kernel validates the request, performs the operation, and returns a file descriptor or an error code. You explored syscalls in [Operating Systems and Linux](/learn/first-principles/operating-systems-and-linux/).

**Web APIs** define communication between components over a network, typically using HTTP. When your application sends `GET /api/users/42` to a remote server, the server processes the request and returns a response — usually JSON — over HTTP. Web APIs are the primary focus of this domain.

APIs are also categorized by their intended audience:

| Category | Audience | Example |
|----------|----------|---------|
| Internal (private) | Teams within the same organization | Microservice-to-microservice calls |
| Partner | Trusted external organizations | Supplier inventory integration |
| Public (open) | Any developer | GitHub API, Stripe API, OpenWeatherMap |

The distinction matters for security, versioning, and documentation. Public APIs demand rigorous backward compatibility because you cannot coordinate upgrades with unknown consumers. Internal APIs can iterate faster because you control both sides.

### Practice

Observe APIs at every level on your system:

```bash
# Library API: Python's os module
python3 -c "import os; print(os.getpid())"

# Syscall API: trace the system calls behind a simple command
strace -c ls /tmp 2>&1 | head -20

# Web API: query a public API
curl -s https://httpbin.org/get | python3 -m json.tool
```

The `strace` output shows every syscall — `openat`, `read`, `write`, `close` — that the `ls` command invokes. The `curl` command sends an HTTP GET request and receives a JSON response. Both are APIs. The difference is the transport layer.

### Connection

Understanding that APIs are contracts — not just HTTP endpoints — changes how you think about system integration. A well-designed contract makes integration straightforward. A poorly designed one creates coupling, brittleness, and debugging nightmares. Every topic in this domain builds on this principle: the API contract is the foundation.

> **Try It**: Run `strace -e trace=network curl -s https://httpbin.org/get > /dev/null` and observe the network-related syscalls. You will see `socket`, `connect`, `sendto`, and `recvfrom` — the syscall API that implements the web API call.

---

## B. REST

### Theory

REST (Representational State Transfer) is an architectural style for web APIs, defined by Roy Fielding in his 2000 doctoral dissertation. REST is not a protocol or a standard. It is a set of constraints that, when followed, produce systems with specific desirable properties: scalability, simplicity, and independent evolvability.

**The six REST constraints:**

1. **Client-server**: The client and server are separate components with distinct responsibilities. The client handles the user interface. The server handles data storage and business logic. This separation allows them to evolve independently.

2. **Stateless**: Each request from client to server must contain all information needed to understand and process the request. The server does not store client session state between requests. This simplifies server design and enables horizontal scaling — any server instance can handle any request.

3. **Cacheable**: Responses must declare whether they are cacheable or not. When a response is cacheable, clients and intermediaries can reuse it for equivalent future requests, reducing latency and server load.

4. **Uniform interface**: The API uses a consistent, standardized interface. This is the most distinctive REST constraint and has four sub-constraints:
   - **Resource identification**: Resources are identified by URIs (`/users/42`)
   - **Resource manipulation through representations**: Clients manipulate resources by sending representations (JSON, XML) with enough information for the server to modify or delete the resource
   - **Self-descriptive messages**: Each message contains enough metadata (content type, cache directives) to describe how to process it
   - **HATEOAS** (Hypermedia as the Engine of Application State): Responses include links that tell the client what actions are available next

5. **Layered system**: The architecture can include intermediary layers (load balancers, caches, gateways) between client and server. The client cannot tell whether it is connected directly to the server or through intermediaries.

6. **Code on demand (optional)**: Servers can extend client functionality by sending executable code (e.g., JavaScript). This is the only optional constraint.

**Resources as nouns.** In REST, URLs identify resources — nouns, not verbs. `/users` is a collection resource. `/users/42` is a specific resource. You never see `/getUser` or `/createUser` in a properly designed REST API. The HTTP method specifies the action.

**HTTP methods map to CRUD operations:**

| HTTP Method | CRUD Operation | Idempotent | Safe | Example |
|-------------|---------------|------------|------|---------|
| GET | Read | Yes | Yes | `GET /users/42` |
| POST | Create | No | No | `POST /users` |
| PUT | Update (full replace) | Yes | No | `PUT /users/42` |
| PATCH | Update (partial) | No* | No | `PATCH /users/42` |
| DELETE | Delete | Yes | No | `DELETE /users/42` |

*PATCH can be made idempotent depending on the patch format.

**Idempotency** means that making the same request multiple times produces the same result as making it once. GET, PUT, and DELETE are idempotent by definition. POST is not — sending `POST /users` twice creates two users. Idempotency matters enormously for reliability: if a network failure hides the server's response, the client can safely retry an idempotent request without fear of duplication.

**HATEOAS** is the most misunderstood and least implemented REST constraint. A HATEOAS response includes hyperlinks that guide the client to available next actions:

```json
{
  "id": 42,
  "name": "Alice",
  "email": "alice@example.com",
  "_links": {
    "self": { "href": "/users/42" },
    "orders": { "href": "/users/42/orders" },
    "update": { "href": "/users/42", "method": "PUT" },
    "delete": { "href": "/users/42", "method": "DELETE" }
  }
}
```

With HATEOAS, the client discovers available actions from the response itself rather than hardcoding URL patterns. In practice, very few APIs implement HATEOAS fully.

**The Richardson Maturity Model** classifies APIs by how closely they follow REST constraints:

| Level | Description | Example |
|-------|-------------|---------|
| 0 | Single URI, single HTTP method (usually POST) | SOAP/XML-RPC: `POST /api` with action in body |
| 1 | Multiple URIs (resources) but single HTTP method | `POST /users`, `POST /users/42` |
| 2 | Multiple URIs + correct HTTP methods | `GET /users/42`, `DELETE /users/42` |
| 3 | Level 2 + HATEOAS (hypermedia controls) | Responses include navigation links |

Most production APIs operate at Level 2. Level 3 is rare. Many APIs described as "RESTful" are actually Level 1 or Level 2.

### Practice

Interact with a REST API and observe the constraints:

```bash
# GET a resource (safe, idempotent)
curl -s -i https://jsonplaceholder.typicode.com/posts/1

# Observe response headers: Content-Type, Cache-Control
curl -s -D - https://jsonplaceholder.typicode.com/posts/1 -o /dev/null

# POST to create a resource (not idempotent)
curl -s -X POST https://jsonplaceholder.typicode.com/posts \
  -H "Content-Type: application/json" \
  -d '{"title":"Test","body":"Content","userId":1}' | python3 -m json.tool

# PUT to replace a resource (idempotent)
curl -s -X PUT https://jsonplaceholder.typicode.com/posts/1 \
  -H "Content-Type: application/json" \
  -d '{"id":1,"title":"Updated","body":"New content","userId":1}' | python3 -m json.tool

# DELETE a resource (idempotent)
curl -s -X DELETE https://jsonplaceholder.typicode.com/posts/1 -w "\nHTTP Status: %{http_code}\n"
```

### Connection

REST is not the only way to build web APIs, but it is the dominant one. When you interact with AWS, Azure, GCP, GitHub, Kubernetes, or virtually any cloud service, you are using REST APIs (or APIs that borrow heavily from REST conventions). Understanding REST constraints helps you evaluate API quality: a truly RESTful API is predictable, cacheable, and scalable. An API that calls itself REST but violates the constraints is harder to use and reason about.

> **Try It**: Evaluate whether the GitHub API is truly RESTful. Run `curl -s https://api.github.com/ | python3 -m json.tool` and examine the response. Does it include HATEOAS-style links? What Richardson Maturity level does it achieve?

---

## C. Data Formats

### Theory

APIs exchange data using serialization formats. The format defines how structured data is encoded as text (or bytes) for transmission and decoded on the other end. Four formats dominate the API landscape.

**JSON (JavaScript Object Notation)** is the standard format for web APIs. It supports six types: string, number, boolean, null, array, and object.

```json
{
  "name": "Alice",
  "age": 30,
  "active": true,
  "address": null,
  "roles": ["admin", "user"],
  "metadata": {
    "created": "2025-01-15T10:30:00Z"
  }
}
```

JSON pitfalls to know:
- **No comments.** JSON does not support comments. JSONC (JSON with Comments) and JSON5 are extensions, not standard JSON.
- **No trailing commas.** `{"a": 1,}` is invalid JSON.
- **Number precision.** JSON numbers are IEEE 754 doubles. Integers larger than 2^53 lose precision. Some APIs send large IDs as strings for this reason.
- **No date type.** Dates are strings. ISO 8601 (`2025-01-15T10:30:00Z`) is the convention, but there is no enforcement.
- **Key order is not guaranteed.** `{"a":1,"b":2}` and `{"b":2,"a":1}` are semantically identical.

**YAML (YAML Ain't Markup Language)** is a superset of JSON used extensively in configuration files (Kubernetes manifests, Ansible playbooks, GitHub Actions, Docker Compose).

```yaml
name: Alice
age: 30
active: true
address: null
roles:
  - admin
  - user
metadata:
  created: "2025-01-15T10:30:00Z"
```

YAML pitfalls — the "Norway problem" and implicit typing:
- YAML auto-interprets values. `yes`, `no`, `on`, `off`, `true`, `false` are all booleans. The country code `NO` (Norway) becomes `false` unless quoted.
- `1.0` is a float, not the string "1.0". `1_000` is the integer 1000.
- Version numbers like `3.10` become the float `3.1`. Always quote version strings: `"3.10"`.
- Indentation must use spaces, never tabs. Inconsistent indentation causes silent parsing errors.

```yaml
# The Norway problem
countries:
  - GB    # string "GB"
  - NO    # boolean false!
  - "NO"  # string "NO" (correct)

# Version number problem
python: 3.10    # float 3.1!
python: "3.10"  # string "3.10" (correct)
```

**XML (Extensible Markup Language)** is the legacy format still found in enterprise systems, SOAP APIs, RSS feeds, and Maven configurations.

```xml
<?xml version="1.0" encoding="UTF-8"?>
<user>
  <name>Alice</name>
  <age>30</age>
  <active>true</active>
  <roles>
    <role>admin</role>
    <role>user</role>
  </roles>
</user>
```

XML is verbose but has features JSON lacks: namespaces, schemas (XSD), transformations (XSLT), and validation. SOAP (Simple Object Access Protocol) is an XML-based web service protocol that was dominant before REST. You will encounter SOAP in legacy enterprise integrations.

**Protocol Buffers (protobuf)** is Google's binary serialization format used by gRPC. Unlike JSON and XML, protobuf requires a schema definition:

```protobuf
syntax = "proto3";

message User {
  int32 id = 1;
  string name = 2;
  string email = 3;
  repeated string roles = 4;
}
```

Protobuf is smaller and faster to parse than JSON. The tradeoff is that messages are not human-readable without the schema definition.

**Parsing JSON with jq.** `jq` is the standard command-line JSON processor. It takes JSON input and applies filters to extract, transform, and format data.

```bash
# Extract a single field
echo '{"name":"Alice","age":30}' | jq '.name'
# Output: "Alice"

# Extract raw string (no quotes)
echo '{"name":"Alice","age":30}' | jq -r '.name'
# Output: Alice

# Access array elements
echo '["a","b","c"]' | jq '.[0]'
# Output: "a"

# Access nested fields
echo '{"user":{"name":"Alice"}}' | jq '.user.name'
# Output: "Alice"

# Filter arrays with select
echo '[{"name":"Alice","age":30},{"name":"Bob","age":25}]' | jq '.[] | select(.age > 27)'
# Output: {"name":"Alice","age":30}

# Transform with map
echo '[1,2,3,4,5]' | jq '[.[] | . * 2]'
# Output: [2,4,6,8,10]

# Convert to CSV
echo '[{"name":"Alice","age":30},{"name":"Bob","age":25}]' | jq -r '.[] | [.name, .age] | @csv'
# Output:
# "Alice",30
# "Bob",25

# Extract keys
echo '{"name":"Alice","age":30}' | jq 'keys'
# Output: ["age","name"]

# Construct new objects
echo '{"first":"Alice","last":"Smith","age":30}' | jq '{full_name: (.first + " " + .last), age}'
# Output: {"full_name":"Alice Smith","age":30}
```

**Parsing YAML with yq.** `yq` applies `jq`-like syntax to YAML files:

```bash
# Read a field from a YAML file
yq '.metadata.name' deployment.yaml

# Convert YAML to JSON
yq -o=json '.' config.yaml

# Update a field in place
yq -i '.spec.replicas = 3' deployment.yaml
```

### Practice

Work with JSON data from a real API:

```bash
# Fetch posts and extract titles
curl -s https://jsonplaceholder.typicode.com/posts | jq -r '.[0:5] | .[].title'

# Fetch users and build a CSV of name and email
curl -s https://jsonplaceholder.typicode.com/users | jq -r '.[] | [.name, .email] | @csv'

# Filter posts by a specific user
curl -s https://jsonplaceholder.typicode.com/posts | jq '[.[] | select(.userId == 1)] | length'

# Nested access: get city from address
curl -s https://jsonplaceholder.typicode.com/users/1 | jq -r '.address.city'
```

Parse JSON in Python:

```python
import json

# Parse a JSON string
data = json.loads('{"name": "Alice", "roles": ["admin", "user"]}')
print(data["name"])        # Alice
print(data["roles"][0])    # admin

# Serialize to JSON
output = json.dumps(data, indent=2)
print(output)

# Handle missing keys safely
email = data.get("email", "not provided")
print(email)  # not provided
```

Parse YAML in Python:

```python
import yaml

# Parse YAML
config = yaml.safe_load("""
server:
  host: 0.0.0.0
  port: 8080
  debug: false
replicas: 3
""")
print(config["server"]["port"])  # 8080

# Serialize to YAML
print(yaml.dump(config, default_flow_style=False))
```

Always use `yaml.safe_load()`, never `yaml.load()`. The unsafe loader can execute arbitrary Python code embedded in YAML — a serious security vulnerability.

### Connection

Data formats are the language of API communication. JSON dominates web APIs. YAML dominates configuration. XML persists in legacy systems. Protobuf powers high-performance RPC. Knowing how to parse and generate all four — and understanding the pitfalls of each — is essential for any integration work. The `jq` tool alone will save you hours of Python scripting when you need to quickly extract data from API responses in a shell pipeline.

> **Try It**: Create a YAML file that demonstrates the Norway problem. Parse it with `python3 -c "import yaml; print(yaml.safe_load(open('test.yaml')))"` and observe how `NO` becomes `False`. Then fix it by quoting the value and verify the fix.

---

## D. Making API Calls

### Theory

Two tools dominate API interaction: `curl` for the command line and Python `requests` for scripts and applications. Both send HTTP requests and receive HTTP responses. Understanding their options deeply means you can interact with any API.

**curl deep dive.** `curl` (Client URL) transfers data using various protocols. For API work, its HTTP capabilities are what matter.

| Flag | Purpose | Example |
|------|---------|---------|
| `-X METHOD` | Set HTTP method | `-X POST`, `-X DELETE` |
| `-H "header"` | Add request header | `-H "Authorization: Bearer token123"` |
| `-d 'data'` | Send request body (sets POST by default) | `-d '{"name":"Alice"}'` |
| `-v` | Verbose output (shows request/response headers) | Debug connection issues |
| `-i` | Include response headers in output | See status codes, content types |
| `-L` | Follow redirects | Handle 301/302 responses |
| `-s` | Silent mode (no progress bar) | Use in scripts |
| `-o file` | Write output to file | `-o response.json` |
| `-w format` | Custom output format | `-w "\n%{http_code}\n"` |
| `--data-binary @file` | Send file contents as body | Upload binary data |
| `-u user:pass` | Basic authentication | `-u admin:secret` |
| `-k` | Skip TLS verification (dangerous) | Testing with self-signed certs |
| `--connect-timeout N` | Connection timeout in seconds | `--connect-timeout 5` |
| `--max-time N` | Total operation timeout | `--max-time 30` |

Key `curl` patterns:

```bash
# GET with headers
curl -s -H "Accept: application/json" https://api.example.com/users

# POST JSON data
curl -s -X POST https://api.example.com/users \
  -H "Content-Type: application/json" \
  -H "Authorization: Bearer mytoken" \
  -d '{"name":"Alice","email":"alice@example.com"}'

# Debug: see full request and response
curl -v https://api.example.com/users 2>&1

# Follow redirects silently, get just the status code
curl -s -L -o /dev/null -w "%{http_code}" https://example.com

# Upload a file
curl -X POST https://api.example.com/upload \
  -H "Content-Type: application/octet-stream" \
  --data-binary @myfile.tar.gz

# Send form data
curl -X POST https://api.example.com/login \
  -d "username=admin&password=secret"
```

**Python `requests` library.** The `requests` library is the standard for HTTP in Python. It provides a clean, consistent interface over the complexities of HTTP.

```python
import requests

# GET request
response = requests.get("https://api.example.com/users")
print(response.status_code)  # 200
print(response.json())       # Parsed JSON as dict/list

# POST with JSON body
response = requests.post(
    "https://api.example.com/users",
    json={"name": "Alice", "email": "alice@example.com"},
    headers={"Authorization": "Bearer mytoken"}
)

# PUT to update
response = requests.put(
    "https://api.example.com/users/42",
    json={"name": "Alice Updated"}
)

# DELETE
response = requests.delete("https://api.example.com/users/42")

# Query parameters
response = requests.get(
    "https://api.example.com/users",
    params={"page": 2, "per_page": 10}
)
# Sends: GET /users?page=2&per_page=10

# Raise an exception on HTTP errors (4xx, 5xx)
response = requests.get("https://api.example.com/users/999")
response.raise_for_status()  # Raises HTTPError if status >= 400

# Timeouts (always set them!)
response = requests.get(
    "https://api.example.com/slow-endpoint",
    timeout=(5, 30)  # (connect_timeout, read_timeout) in seconds
)

# Sessions for connection reuse and persistent headers
session = requests.Session()
session.headers.update({"Authorization": "Bearer mytoken"})
r1 = session.get("https://api.example.com/users")
r2 = session.get("https://api.example.com/posts")  # Reuses connection and headers
```

**HTTPie** is a modern alternative to `curl` with a more intuitive syntax:

```bash
# GET request (JSON output by default, colored)
http https://api.example.com/users

# POST with JSON (automatic Content-Type)
http POST https://api.example.com/users name=Alice email=alice@example.com

# Custom headers
http https://api.example.com/users Authorization:"Bearer mytoken"

# Form data
http --form POST https://api.example.com/login username=admin password=secret
```

HTTPie is excellent for interactive exploration. `curl` is better for scripts because it is installed everywhere.

### Practice

Make real API calls and inspect the responses:

```bash
# Verbose GET to see the full HTTP conversation
curl -v https://jsonplaceholder.typicode.com/posts/1 2>&1 | head -30

# POST and capture the response status
curl -s -w "\nHTTP Status: %{http_code}\n" \
  -X POST https://jsonplaceholder.typicode.com/posts \
  -H "Content-Type: application/json" \
  -d '{"title":"Test Post","body":"Hello API","userId":1}'

# Chain curl with jq for processing
curl -s https://jsonplaceholder.typicode.com/users | \
  jq -r '.[] | "\(.name) <\(.email)>"'
```

Build a complete Python script:

```python
import requests
import sys

def fetch_user_posts(user_id):
    """Fetch all posts for a given user with proper error handling."""
    base_url = "https://jsonplaceholder.typicode.com"

    try:
        # Fetch user info
        user_resp = requests.get(
            f"{base_url}/users/{user_id}",
            timeout=(5, 10)
        )
        user_resp.raise_for_status()
        user = user_resp.json()

        # Fetch user's posts
        posts_resp = requests.get(
            f"{base_url}/posts",
            params={"userId": user_id},
            timeout=(5, 10)
        )
        posts_resp.raise_for_status()
        posts = posts_resp.json()

        print(f"User: {user['name']} ({user['email']})")
        print(f"Posts: {len(posts)}")
        for post in posts:
            print(f"  - {post['title']}")

    except requests.exceptions.ConnectionError:
        print("Error: Could not connect to the API", file=sys.stderr)
        sys.exit(1)
    except requests.exceptions.Timeout:
        print("Error: Request timed out", file=sys.stderr)
        sys.exit(1)
    except requests.exceptions.HTTPError as e:
        print(f"Error: HTTP {e.response.status_code}", file=sys.stderr)
        sys.exit(1)

if __name__ == "__main__":
    fetch_user_posts(1)
```

### Connection

`curl` is your debugging tool. When an API integration breaks, your first move is to reproduce the request with `curl -v` and inspect the raw HTTP conversation. Python `requests` is your integration tool. When you build automation, scripts, and services, `requests` gives you programmatic control. Both use the same HTTP protocol underneath. The flags and parameters are different syntax for the same operations you learned in [Networking](/learn/first-principles/networking/).

> **Try It**: Use `curl -v` to send a POST request to `https://httpbin.org/post` with a JSON body. Read the verbose output carefully: identify the DNS resolution, TCP connection, TLS handshake, HTTP request headers, request body, response status line, response headers, and response body. This is the full lifecycle of an API call.

---

## E. Authentication

### Theory

APIs need to verify who is making a request (authentication) and whether that identity is allowed to perform the requested action (authorization). You studied these concepts in [Security and Cryptography](/learn/first-principles/security-and-cryptography/). Here we apply them to API-specific mechanisms.

**API keys** are the simplest authentication method. The server issues a long random string. The client includes it in every request. API keys are typically sent as a header or query parameter.

```bash
# API key in header (preferred)
curl -H "X-API-Key: abc123def456" https://api.example.com/data

# API key in query parameter (less secure — visible in logs and browser history)
curl "https://api.example.com/data?api_key=abc123def456"
```

API keys identify the application, not the user. They are appropriate for server-to-server communication where a human identity is not needed. They should be treated as secrets — never committed to version control, never included in client-side code.

**Basic authentication** sends a username and password encoded in Base64 in the `Authorization` header.

```bash
# curl handles Basic auth natively
curl -u username:password https://api.example.com/data

# What actually gets sent (Base64 encoding, NOT encryption)
echo -n "username:password" | base64
# dXNlcm5hbWU6cGFzc3dvcmQ=

curl -H "Authorization: Basic dXNlcm5hbWU6cGFzc3dvcmQ=" https://api.example.com/data
```

Base64 is encoding, not encryption. Basic auth credentials are readable by anyone who intercepts the request. Always use Basic auth over HTTPS, never over plain HTTP.

**Bearer tokens and JWT.** A Bearer token is an opaque or structured token sent in the `Authorization` header. The server trusts the bearer of the token without further verification of identity — possession of the token is proof of authorization.

```bash
curl -H "Authorization: Bearer eyJhbGciOiJIUzI1NiIs..." https://api.example.com/data
```

**JSON Web Tokens (JWT)** are the most common type of bearer token. A JWT has three parts separated by dots:

```
header.payload.signature
```

```
eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJzdWIiOiIxMjM0NTY3ODkwIiwibmFtZSI6IkFsaWNlIiwiaWF0IjoxNzA1MzEyMDAwLCJleHAiOjE3MDUzMTU2MDB9.SflKxwRJSMeKKF2QT4fwpMeJf36POk6yJV_adQssw5c
```

Each part is Base64url-encoded:

| Part | Contents | Example |
|------|----------|---------|
| Header | Algorithm and token type | `{"alg":"HS256","typ":"JWT"}` |
| Payload | Claims (data) | `{"sub":"1234567890","name":"Alice","iat":1705312000,"exp":1705315600}` |
| Signature | HMAC or RSA signature over header + payload | Prevents tampering |

Standard JWT claims:

| Claim | Name | Purpose |
|-------|------|---------|
| `sub` | Subject | Who the token is about (user ID) |
| `iss` | Issuer | Who created the token |
| `aud` | Audience | Who the token is intended for |
| `iat` | Issued At | When the token was created (Unix timestamp) |
| `exp` | Expiration | When the token expires (Unix timestamp) |
| `nbf` | Not Before | Token is not valid before this time |

The signature prevents tampering. If anyone modifies the header or payload, the signature will not match, and the server will reject the token. However, the payload is only encoded, not encrypted — anyone can read the claims by decoding the Base64url. Never put sensitive data in JWT payloads.

```bash
# Decode a JWT payload (second segment)
echo "eyJzdWIiOiIxMjM0NTY3ODkwIiwibmFtZSI6IkFsaWNlIiwiaWF0IjoxNzA1MzEyMDAwfQ" | \
  base64 -d 2>/dev/null | python3 -m json.tool
```

**Token expiration** is a critical security feature. Short-lived access tokens (minutes to hours) limit the damage if a token is stolen. Refresh tokens (longer-lived) allow obtaining new access tokens without re-authenticating.

**OAuth 2.0** is an authorization framework that allows third-party applications to access resources on behalf of a user without knowing the user's password. OAuth 2.0 defines several grant types (flows) for different scenarios.

```mermaid
sequenceDiagram
    participant User
    participant App as Client App
    participant Auth as Auth Server
    participant API as Resource Server

    Note over User,API: Authorization Code Flow
    User->>App: Click "Login with GitHub"
    App->>Auth: Redirect to /authorize
    Auth->>User: Show consent screen
    User->>Auth: Grant permission
    Auth->>App: Redirect with authorization code
    App->>Auth: Exchange code for token (with client secret)
    Auth->>App: Access token (+ refresh token)
    App->>API: Request with access token
    API->>App: Protected resource
```

**OAuth 2.0 grant types:**

| Grant Type | Use Case | Client Type |
|-----------|----------|-------------|
| Authorization Code | Web apps, mobile apps with backend | Confidential or public |
| Client Credentials | Server-to-server (no user context) | Confidential |
| Device Code | Smart TVs, CLI tools (limited input) | Public |
| Refresh Token | Obtain new access token without re-auth | Any |

**Authorization Code flow** is the most common. It involves redirecting the user to the authorization server, getting consent, receiving a code, and exchanging that code for tokens. The code exchange happens server-side, keeping the client secret secure.

**Client Credentials flow** is for machine-to-machine communication where no user is involved. The client authenticates directly with its client ID and secret to get an access token.

```python
import requests

# Client Credentials flow
token_response = requests.post(
    "https://auth.example.com/oauth/token",
    data={
        "grant_type": "client_credentials",
        "client_id": "my-app-id",
        "client_secret": "my-app-secret",
        "scope": "read:users write:users"
    }
)
access_token = token_response.json()["access_token"]

# Use the token
response = requests.get(
    "https://api.example.com/users",
    headers={"Authorization": f"Bearer {access_token}"}
)
```

**Device Code flow** is for devices with limited input capabilities. The device displays a code and URL. The user enters the code on a different device (phone, laptop) to authorize.

**Scopes** limit what an access token can do. Instead of granting full access, the token is restricted to specific operations: `read:users`, `write:repos`, `admin:org`. The principle of least privilege applied to API access.

**Service accounts** are non-human identities used by automated systems. Instead of a person's credentials, a service account has its own API key, certificate, or OAuth credentials. Cloud providers (AWS IAM roles, GCP service accounts, Azure managed identities) provide service account mechanisms specifically designed for workloads running in their environments.

### Practice

Decode and inspect a JWT:

```python
import base64
import json

def decode_jwt(token):
    """Decode a JWT without verifying the signature."""
    parts = token.split(".")
    if len(parts) != 3:
        raise ValueError("Invalid JWT format")

    # Decode header and payload (add padding if needed)
    header = base64.urlsafe_b64decode(parts[0] + "==")
    payload = base64.urlsafe_b64decode(parts[1] + "==")

    print("Header:")
    print(json.dumps(json.loads(header), indent=2))
    print("\nPayload:")
    print(json.dumps(json.loads(payload), indent=2))

# Example JWT (from jwt.io)
token = "eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJzdWIiOiIxMjM0NTY3ODkwIiwibmFtZSI6IkFsaWNlIiwiaWF0IjoxNzA1MzEyMDAwfQ.SflKxwRJSMeKKF2QT4fwpMeJf36POk6yJV_adQssw5c"
decode_jwt(token)
```

Test authentication methods against httpbin:

```bash
# Basic auth
curl -u user:passwd https://httpbin.org/basic-auth/user/passwd

# Bearer token
curl -H "Authorization: Bearer test-token-123" https://httpbin.org/bearer

# See what headers the server receives
curl -s -H "Authorization: Bearer mytoken" \
  -H "X-API-Key: abc123" \
  https://httpbin.org/headers | jq '.headers'
```

### Connection

Authentication is the gateway to every API interaction. In practice, you will manage API keys in secrets managers (HashiCorp Vault, AWS Secrets Manager), configure service accounts for automated workloads, implement OAuth flows for user-facing applications, and debug JWT expiration issues when tokens stop working at 3 AM. Understanding the mechanisms — not just copying token values — lets you diagnose authentication failures from the error message rather than guessing.

> **Try It**: Go to [jwt.io](https://jwt.io) and paste a JWT. Observe how the header, payload, and signature are decoded visually. Modify a single character in the payload and watch the signature verification fail. This demonstrates why JWTs are tamper-proof.

---

## F. API Design

### Theory

Designing a good API is designing a good contract. The decisions you make about versioning, pagination, error handling, and rate limiting determine whether your API is a pleasure to integrate with or a source of constant frustration.

**Versioning.** APIs evolve. When you change an API in a backward-incompatible way, existing clients break. Versioning strategies let you evolve without breaking consumers.

| Strategy | Example | Pros | Cons |
|----------|---------|------|------|
| URL path | `/v1/users`, `/v2/users` | Simple, explicit, easy to route | URL pollution, hard to deprecate |
| Header | `Accept: application/vnd.api.v2+json` | Clean URLs | Hidden, harder to discover |
| Query parameter | `/users?version=2` | Simple | Easy to forget, caching issues |

URL path versioning is the most common in practice because it is the simplest to implement, document, and debug.

**Pagination.** When a collection has thousands of items, returning all of them in one response is impractical. Three pagination strategies exist:

| Strategy | How It Works | Pros | Cons |
|----------|-------------|------|------|
| Offset | `?offset=20&limit=10` | Simple, random access | Slow on large datasets, inconsistent with concurrent writes |
| Cursor | `?cursor=eyJpZCI6MTAwfQ==` | Consistent, performant | No random access, opaque cursor |
| Keyset | `?after_id=100&limit=10` | Fast (index-based), consistent | Requires sortable key, no random access |

Cursor-based pagination is preferred for large or frequently changing datasets. The response includes a cursor (usually an opaque, Base64-encoded string) that points to the next page:

```json
{
  "data": [...],
  "pagination": {
    "next_cursor": "eyJpZCI6MTAwfQ==",
    "has_more": true
  }
}
```

**Rate limiting.** APIs limit request rates to protect server resources and ensure fair usage. When you exceed the limit, the server returns HTTP 429 (Too Many Requests). Well-designed APIs include rate limit information in response headers:

```
X-RateLimit-Limit: 100
X-RateLimit-Remaining: 23
X-RateLimit-Reset: 1705315600
Retry-After: 30
```

| Header | Meaning |
|--------|---------|
| `X-RateLimit-Limit` | Maximum requests allowed in the window |
| `X-RateLimit-Remaining` | Requests remaining in current window |
| `X-RateLimit-Reset` | Unix timestamp when the window resets |
| `Retry-After` | Seconds to wait before retrying (on 429 responses) |

**Error responses.** A well-designed API returns structured error information, not just a status code.

```json
{
  "error": {
    "code": "VALIDATION_ERROR",
    "message": "The 'email' field must be a valid email address.",
    "details": [
      {
        "field": "email",
        "value": "not-an-email",
        "constraint": "format:email"
      }
    ],
    "request_id": "req_abc123"
  }
}
```

Error response best practices:
- Use appropriate HTTP status codes (400 for client errors, 500 for server errors)
- Include a machine-readable error code (`VALIDATION_ERROR`, not just "Bad Request")
- Include a human-readable message
- Include a request ID for debugging (correlate with server logs)
- Never expose internal implementation details (stack traces, database errors)

**Idempotency keys.** For non-idempotent operations (POST), clients can send an idempotency key — a unique identifier for the request. If the server receives the same idempotency key twice, it returns the original response instead of processing the request again. Stripe popularized this pattern:

```bash
curl -X POST https://api.example.com/payments \
  -H "Idempotency-Key: unique-request-id-123" \
  -H "Content-Type: application/json" \
  -d '{"amount": 1000, "currency": "usd"}'
```

This is critical for financial operations. If the network fails after the server processes the payment but before the client receives the response, the client can safely retry with the same idempotency key without creating a duplicate payment.

**Bulk operations.** When you need to create, update, or delete many resources, making individual API calls is inefficient. Bulk endpoints accept arrays of operations:

```json
{
  "operations": [
    {"method": "POST", "path": "/users", "body": {"name": "Alice"}},
    {"method": "POST", "path": "/users", "body": {"name": "Bob"}},
    {"method": "DELETE", "path": "/users/42"}
  ]
}
```

Some APIs use batch endpoints (`POST /batch`) while others accept arrays at the resource endpoint (`POST /users` with an array body). The design choice depends on whether operations are homogeneous or mixed.

### Practice

Design a RESTful API for a simple task management system:

```
# Resources and endpoints

GET    /v1/tasks                    # List all tasks (paginated)
POST   /v1/tasks                    # Create a task
GET    /v1/tasks/{id}               # Get a specific task
PUT    /v1/tasks/{id}               # Replace a task
PATCH  /v1/tasks/{id}               # Update task fields
DELETE /v1/tasks/{id}               # Delete a task
GET    /v1/tasks/{id}/comments      # List comments on a task
POST   /v1/tasks/{id}/comments      # Add a comment

# Pagination
GET /v1/tasks?cursor=abc123&limit=20

# Filtering
GET /v1/tasks?status=open&assignee=alice&sort=-created_at

# Error response
{
  "error": {
    "code": "NOT_FOUND",
    "message": "Task with ID 999 does not exist.",
    "request_id": "req_7f3b2a"
  }
}
```

Implement pagination handling in Python:

```python
import requests

def fetch_all_pages(base_url, headers=None):
    """Fetch all pages from a cursor-paginated API."""
    all_items = []
    cursor = None

    while True:
        params = {"limit": 100}
        if cursor:
            params["cursor"] = cursor

        response = requests.get(base_url, params=params, headers=headers, timeout=(5, 30))
        response.raise_for_status()
        data = response.json()

        all_items.extend(data["items"])

        if not data.get("has_more"):
            break
        cursor = data["next_cursor"]

    return all_items
```

### Connection

API design decisions have cascading consequences. Poor versioning strategies create breaking changes that disrupt entire ecosystems. Missing pagination causes timeouts on large datasets. Absent rate limiting allows a single client to degrade service for everyone. Lack of idempotency keys causes duplicate operations that are expensive to undo. When you consume APIs, you are affected by these decisions. When you design them, you make them. Understanding the tradeoffs lets you do both well.

> **Try It**: Examine the GitHub API's pagination. Run `curl -s -I "https://api.github.com/repos/kubernetes/kubernetes/commits?per_page=5"` and inspect the `Link` header. GitHub uses RFC 8288 link relations to provide next, prev, first, and last page URLs. Parse the `Link` header to extract the next page URL.

---

## G. Client Resilience

### Theory

Networks fail. Servers crash. Timeouts expire. A resilient API client handles these failures gracefully instead of crashing or corrupting data. Three patterns form the foundation of client resilience.

**Retry logic.** Not all errors are retryable. The decision depends on the HTTP status code:

| Status Code | Meaning | Retry? | Why |
|-------------|---------|--------|-----|
| 400 Bad Request | Client sent invalid data | No | Same request will fail again |
| 401 Unauthorized | Invalid credentials | No | Need new credentials |
| 403 Forbidden | Not permitted | No | Permission issue, not transient |
| 404 Not Found | Resource does not exist | No | Resource will not appear on retry |
| 408 Request Timeout | Server timed out | Yes | Transient server issue |
| 429 Too Many Requests | Rate limited | Yes | Wait and retry after backoff |
| 500 Internal Server Error | Server bug | Yes | May be transient |
| 502 Bad Gateway | Upstream failure | Yes | Usually transient |
| 503 Service Unavailable | Server overloaded | Yes | Transient by definition |
| 504 Gateway Timeout | Upstream timeout | Yes | Usually transient |

The rule: retry 5xx errors (server problems) and 429 (rate limits). Do not retry 4xx errors (client problems) — the same request will produce the same error.

**Exponential backoff with jitter.** Retrying immediately after a failure makes things worse. If the server is overloaded and 1,000 clients all retry at the same time, the thundering herd overwhelms the server again. Exponential backoff spaces out retries with increasing delays. Jitter adds randomness to prevent synchronized retries.

```
Attempt 1: wait 1s  + random(0, 0.5s)
Attempt 2: wait 2s  + random(0, 1.0s)
Attempt 3: wait 4s  + random(0, 2.0s)
Attempt 4: wait 8s  + random(0, 4.0s)
Attempt 5: wait 16s + random(0, 8.0s)  — then give up
```

The formula is: `delay = min(base * 2^attempt, max_delay) + random(0, jitter)`

```python
import time
import random
import requests

def request_with_retry(method, url, max_retries=5, base_delay=1, max_delay=60, **kwargs):
    """Make an HTTP request with exponential backoff and jitter."""
    kwargs.setdefault("timeout", (5, 30))

    for attempt in range(max_retries + 1):
        try:
            response = requests.request(method, url, **kwargs)

            # Don't retry client errors (except 429)
            if 400 <= response.status_code < 500 and response.status_code != 429:
                return response

            # Success — return immediately
            if response.status_code < 400:
                return response

            # Retryable error — fall through to retry logic
            if attempt == max_retries:
                return response  # Final attempt, return whatever we got

        except (requests.exceptions.ConnectionError, requests.exceptions.Timeout) as e:
            if attempt == max_retries:
                raise

        # Calculate delay with exponential backoff and jitter
        delay = min(base_delay * (2 ** attempt), max_delay)
        jitter = random.uniform(0, delay * 0.5)
        total_delay = delay + jitter

        # Honor Retry-After header if present
        if 'response' in dir() and response is not None:
            retry_after = response.headers.get("Retry-After")
            if retry_after:
                total_delay = max(total_delay, float(retry_after))

        print(f"Attempt {attempt + 1} failed. Retrying in {total_delay:.1f}s...")
        time.sleep(total_delay)

    return response
```

**Circuit breaker.** A circuit breaker prevents a client from repeatedly calling a service that is known to be down. It tracks failures and, after a threshold is reached, "opens" the circuit — immediately returning an error without making the request. After a cooldown period, it allows a single test request through. If that succeeds, the circuit closes and normal operation resumes.

```mermaid
stateDiagram-v2
    [*] --> Closed
    Closed --> Open : Failure threshold exceeded
    Open --> HalfOpen : Cooldown timer expires
    HalfOpen --> Closed : Test request succeeds
    HalfOpen --> Open : Test request fails
```

```python
import time

class CircuitBreaker:
    def __init__(self, failure_threshold=5, reset_timeout=60):
        self.failure_threshold = failure_threshold
        self.reset_timeout = reset_timeout
        self.failure_count = 0
        self.state = "closed"  # closed, open, half-open
        self.last_failure_time = None

    def call(self, func, *args, **kwargs):
        if self.state == "open":
            if time.time() - self.last_failure_time >= self.reset_timeout:
                self.state = "half-open"
            else:
                raise Exception("Circuit breaker is open")

        try:
            result = func(*args, **kwargs)
            if self.state == "half-open":
                self.state = "closed"
                self.failure_count = 0
            return result
        except Exception as e:
            self.failure_count += 1
            self.last_failure_time = time.time()
            if self.failure_count >= self.failure_threshold:
                self.state = "open"
            raise
```

**Timeouts.** Two types of timeouts exist, and confusing them causes production incidents:

| Timeout Type | What It Measures | Typical Value |
|-------------|-----------------|---------------|
| Connect timeout | Time to establish TCP connection | 3-5 seconds |
| Read timeout | Time to receive first byte of response | 10-30 seconds |

A missing timeout is a bug. If the server hangs, a request without a timeout will block forever, consuming resources and eventually cascading into a system-wide failure.

```python
# Always set both timeouts
response = requests.get(
    "https://api.example.com/data",
    timeout=(5, 30)  # 5s to connect, 30s to read
)
```

### Practice

Test retry behavior against a service that sometimes fails:

```bash
# httpbin can simulate failures
# Returns 500 status
curl -s -w "\nHTTP %{http_code}\n" https://httpbin.org/status/500

# Returns 200 status
curl -s -w "\nHTTP %{http_code}\n" https://httpbin.org/status/200

# Simulates delay
curl -s --max-time 3 https://httpbin.org/delay/5
# This will timeout after 3 seconds
```

Build a resilient client:

```python
import requests
import time
import random

def resilient_get(url, max_retries=3):
    """Demonstrate retry with exponential backoff."""
    for attempt in range(max_retries + 1):
        try:
            response = requests.get(url, timeout=(5, 10))
            print(f"Attempt {attempt + 1}: HTTP {response.status_code}")

            if response.status_code < 400:
                return response
            if 400 <= response.status_code < 500 and response.status_code != 429:
                print(f"Client error {response.status_code}, not retrying")
                return response

        except requests.exceptions.Timeout:
            print(f"Attempt {attempt + 1}: Timeout")
        except requests.exceptions.ConnectionError:
            print(f"Attempt {attempt + 1}: Connection failed")

        if attempt < max_retries:
            delay = (2 ** attempt) + random.uniform(0, 1)
            print(f"Waiting {delay:.1f}s before retry...")
            time.sleep(delay)

    print("All retries exhausted")
    return None

# Test against a working endpoint
resilient_get("https://httpbin.org/get")

# Test against a failing endpoint
resilient_get("https://httpbin.org/status/503")
```

### Connection

Client resilience is not optional in production systems. Network failures, server restarts, deployment rollouts, and traffic spikes all cause transient errors. The difference between a system that self-heals and one that cascades into an outage is often just retry logic with exponential backoff. Every SDK, CLI, and automation tool you build should implement these patterns. Every API you call in [Infrastructure at Scale](/learn/first-principles/infrastructure-at-scale/) — Kubernetes API, cloud provider APIs, container registries — benefits from resilient client behavior.

> **Try It**: Write a Python script that makes requests to `https://httpbin.org/status/503` with exponential backoff. Log each attempt with the delay. Verify that delays follow the pattern 1s, 2s, 4s, 8s (approximately, with jitter). Then modify the script to cap the maximum delay at 10 seconds.

---

## H. Beyond REST

### Theory

REST is dominant but not universal. Several alternative API paradigms exist, each optimized for specific use cases.

**GraphQL** is a query language for APIs developed by Facebook (2015). Instead of multiple endpoints returning fixed data structures, GraphQL exposes a single endpoint. The client specifies exactly what data it needs.

REST approach (multiple requests):
```
GET /users/42
GET /users/42/posts
GET /users/42/followers
```

GraphQL approach (single request):
```graphql
query {
  user(id: 42) {
    name
    email
    posts {
      title
      createdAt
    }
    followers {
      name
    }
  }
}
```

The request is a POST to a single endpoint (typically `/graphql`) with the query in the body:

```bash
curl -X POST https://api.example.com/graphql \
  -H "Content-Type: application/json" \
  -d '{"query": "{ user(id: 42) { name email } }"}'
```

GraphQL advantages:
- **No over-fetching**: The client gets exactly the fields it requests
- **No under-fetching**: One request can traverse relationships that would require multiple REST calls
- **Strong typing**: The schema defines every type, field, and relationship
- **Introspection**: Clients can query the schema itself to discover available data

GraphQL challenges:
- **N+1 problem**: A naive server implementation fetches related data one record at a time. If a query requests 100 users and each user's posts, the server makes 1 query for users + 100 queries for posts. Solutions include DataLoader (batching) and query planning.
- **Caching complexity**: REST caches use URL as the cache key. GraphQL uses POST with varying query bodies, making HTTP caching harder.
- **Query complexity**: Without limits, a malicious client can send deeply nested queries that consume excessive server resources.

**gRPC** (gRPC Remote Procedure Call) is a high-performance RPC framework from Google. It uses Protocol Buffers for serialization and HTTP/2 for transport.

Define the service in a `.proto` file:

```protobuf
syntax = "proto3";

service UserService {
  rpc GetUser (GetUserRequest) returns (User);
  rpc ListUsers (ListUsersRequest) returns (stream User);  // Server streaming
}

message GetUserRequest {
  int32 id = 1;
}

message User {
  int32 id = 1;
  string name = 2;
  string email = 3;
}
```

gRPC advantages:
- **Performance**: Binary protobuf is 3-10x smaller and faster to parse than JSON
- **HTTP/2**: Multiplexed streams, header compression, bidirectional streaming
- **Strong contracts**: The `.proto` file is the definitive API specification
- **Code generation**: Client and server stubs are generated from the `.proto` file
- **Streaming**: Four patterns — unary, server streaming, client streaming, bidirectional streaming

gRPC is ideal for microservice-to-microservice communication where performance matters. It is not ideal for browser-based clients (limited browser support for HTTP/2 trailers) or public APIs (tooling is less mature than REST).

| Feature | REST | GraphQL | gRPC |
|---------|------|---------|------|
| Transport | HTTP/1.1 or HTTP/2 | HTTP | HTTP/2 |
| Data format | JSON (typically) | JSON | Protocol Buffers (binary) |
| Schema | Optional (OpenAPI) | Required (SDL) | Required (.proto) |
| Endpoints | Multiple | Single | Per-service |
| Browser support | Native | Native (via HTTP) | Limited (needs gRPC-Web) |
| Streaming | Not native | Subscriptions | Native (4 patterns) |
| Caching | HTTP caching | Complex | Not HTTP-cacheable |
| Best for | Public APIs, CRUD | Complex data graphs | High-performance internal |

**WebSockets** provide full-duplex, bidirectional communication over a single TCP connection. Unlike HTTP (request-response), WebSockets allow both client and server to send messages at any time.

```mermaid
sequenceDiagram
    participant Client
    participant Server

    Note over Client,Server: HTTP → WebSocket Upgrade
    Client->>Server: GET /ws (Upgrade: websocket)
    Server->>Client: 101 Switching Protocols

    Note over Client,Server: Full-duplex communication
    Client->>Server: Message
    Server->>Client: Message
    Server->>Client: Message
    Client->>Server: Message
    Client->>Server: Close
```

The connection starts as an HTTP request with an `Upgrade` header. Once the server accepts, the protocol switches from HTTP to WebSocket. From that point, both sides can send messages independently.

```python
# Python WebSocket client (requires 'websockets' library)
import asyncio
import websockets

async def connect():
    async with websockets.connect("wss://echo.websockets.events") as ws:
        await ws.send("Hello, WebSocket!")
        response = await ws.recv()
        print(f"Received: {response}")

asyncio.run(connect())
```

WebSockets are appropriate for:
- Real-time applications (chat, gaming, live dashboards)
- Streaming data (stock tickers, log tailing)
- Bidirectional communication (collaborative editing)

WebSockets are not appropriate for:
- Simple request-response patterns (use REST)
- Occasional updates (use polling or SSE)
- Situations where HTTP caching is needed

**Webhooks** are the reverse of polling. Instead of the client repeatedly asking "has anything changed?", the server sends an HTTP POST to a client-specified URL when an event occurs.

```mermaid
sequenceDiagram
    participant App as Your App
    participant Service as GitHub

    Note over App,Service: Registration
    App->>Service: Register webhook URL: https://myapp.com/hooks/github

    Note over App,Service: Later, when an event occurs...
    Service->>App: POST /hooks/github {"event": "push", "repo": "..."}
    App->>Service: 200 OK (acknowledge receipt)
```

Webhook security considerations:
- **Signature verification**: The sender signs the payload with a shared secret. The receiver verifies the signature before processing.
- **Replay protection**: Include a timestamp; reject old webhooks.
- **Idempotency**: Webhooks can be delivered multiple times. Your handler must be idempotent.

```python
import hmac
import hashlib

def verify_github_webhook(payload_body, signature_header, secret):
    """Verify that a GitHub webhook payload is authentic."""
    expected_signature = "sha256=" + hmac.new(
        secret.encode("utf-8"),
        payload_body,
        hashlib.sha256
    ).hexdigest()
    return hmac.compare_digest(expected_signature, signature_header)
```

**Server-Sent Events (SSE)** are a lightweight, unidirectional streaming mechanism. The server sends events to the client over a standard HTTP connection. Unlike WebSockets, SSE is one-way (server to client only) and uses plain HTTP, making it simpler to implement and compatible with existing infrastructure (load balancers, proxies, CDNs).

```bash
# SSE endpoint returns a stream of events
curl -N https://api.example.com/events/stream
# data: {"type": "update", "value": 42}
#
# data: {"type": "update", "value": 43}
#
# data: {"type": "update", "value": 44}
```

| Paradigm | Direction | Connection | Best For |
|----------|-----------|-----------|----------|
| REST | Request-response | Short-lived | CRUD, public APIs |
| GraphQL | Request-response | Short-lived | Complex data requirements |
| gRPC | Bidirectional | Persistent (HTTP/2) | High-performance services |
| WebSockets | Bidirectional | Persistent | Real-time interaction |
| Webhooks | Server-to-client (push) | Per-event | Event notifications |
| SSE | Server-to-client (stream) | Persistent | Live feeds, monitoring |

### Practice

Explore GraphQL with a public API:

```bash
# Query the GitHub GraphQL API (requires a personal access token)
curl -s -X POST https://api.github.com/graphql \
  -H "Authorization: Bearer YOUR_TOKEN" \
  -H "Content-Type: application/json" \
  -d '{"query": "{ viewer { login name repositories(first: 5) { nodes { name stargazerCount } } } }"}' | \
  python3 -m json.tool
```

Test Server-Sent Events:

```python
import requests

def listen_sse(url):
    """Listen to a Server-Sent Events stream."""
    response = requests.get(url, stream=True, timeout=(5, None))
    for line in response.iter_lines():
        if line:
            decoded = line.decode("utf-8")
            if decoded.startswith("data: "):
                print(f"Event: {decoded[6:]}")

# Example SSE endpoint (replace with a real one for testing)
# listen_sse("https://api.example.com/events/stream")
```

### Connection

Choosing the right API paradigm is an architectural decision with long-term consequences. REST is the default for most use cases. GraphQL shines when clients have diverse data requirements and over-fetching is costly (mobile apps with limited bandwidth). gRPC is ideal for internal microservice communication where performance is critical. WebSockets are necessary for real-time, bidirectional interaction. Webhooks replace polling for event-driven architectures. SSE provides a simpler alternative to WebSockets when you only need server-to-client updates. In [Infrastructure at Scale](/learn/first-principles/infrastructure-at-scale/), you will encounter all of these: Kubernetes uses a REST API with watch streams (similar to SSE), Prometheus uses HTTP pull, and service meshes use gRPC between sidecars.

> **Try It**: Compare the REST and GraphQL approaches by fetching your GitHub profile. First use the REST API: `curl -s https://api.github.com/users/YOUR_USERNAME | jq '.name, .public_repos, .followers'`. Then use the GraphQL API to request the same data in a single query. Note the difference in response sizes.

---

## I. API Documentation

### Theory

Good documentation is the difference between an API that gets adopted and one that gets abandoned. The OpenAPI Specification (formerly Swagger) is the industry standard for describing REST APIs.

**OpenAPI Specification (OAS)** is a machine-readable description of a REST API written in YAML or JSON. It defines endpoints, methods, parameters, request bodies, responses, authentication, and schemas.

```yaml
openapi: 3.0.3
info:
  title: Task Management API
  description: A simple task management REST API
  version: 1.0.0

servers:
  - url: https://api.example.com/v1
    description: Production

paths:
  /tasks:
    get:
      summary: List all tasks
      operationId: listTasks
      parameters:
        - name: status
          in: query
          schema:
            type: string
            enum: [open, closed, in_progress]
        - name: limit
          in: query
          schema:
            type: integer
            default: 20
            maximum: 100
      responses:
        "200":
          description: A list of tasks
          content:
            application/json:
              schema:
                type: object
                properties:
                  items:
                    type: array
                    items:
                      $ref: "#/components/schemas/Task"
                  has_more:
                    type: boolean
        "401":
          description: Unauthorized

    post:
      summary: Create a task
      operationId: createTask
      requestBody:
        required: true
        content:
          application/json:
            schema:
              $ref: "#/components/schemas/CreateTask"
      responses:
        "201":
          description: Task created
          content:
            application/json:
              schema:
                $ref: "#/components/schemas/Task"
        "400":
          description: Invalid input

  /tasks/{taskId}:
    get:
      summary: Get a task by ID
      operationId: getTask
      parameters:
        - name: taskId
          in: path
          required: true
          schema:
            type: string
      responses:
        "200":
          description: A single task
          content:
            application/json:
              schema:
                $ref: "#/components/schemas/Task"
        "404":
          description: Task not found

components:
  schemas:
    Task:
      type: object
      properties:
        id:
          type: string
        title:
          type: string
        status:
          type: string
          enum: [open, closed, in_progress]
        created_at:
          type: string
          format: date-time
      required: [id, title, status, created_at]

    CreateTask:
      type: object
      properties:
        title:
          type: string
          minLength: 1
          maxLength: 200
        description:
          type: string
      required: [title]

  securitySchemes:
    bearerAuth:
      type: http
      scheme: bearer
      bearerFormat: JWT

security:
  - bearerAuth: []
```

**Reading an OpenAPI spec.** The key sections:

| Section | Purpose |
|---------|---------|
| `info` | API name, description, version |
| `servers` | Base URLs for the API |
| `paths` | Endpoints and their operations |
| `components/schemas` | Reusable data type definitions |
| `components/securitySchemes` | Authentication methods |
| `security` | Default security requirements |

Each path operation defines:
- HTTP method and URL
- Parameters (path, query, header, cookie)
- Request body schema
- Response schemas for each status code
- Security requirements

**Swagger UI** renders an OpenAPI spec as an interactive web page where developers can read documentation and make test API calls directly in the browser. Many APIs host their Swagger UI publicly (e.g., Petstore: `https://petstore.swagger.io`).

**API development tools:**

| Tool | Purpose |
|------|---------|
| Swagger UI | Interactive API documentation from OpenAPI specs |
| Swagger Editor | Write and validate OpenAPI specs |
| Postman | GUI for building, testing, and documenting API requests |
| Insomnia | Lightweight alternative to Postman |
| Redocly | Beautiful API documentation generation |
| `openapi-generator` | Generate client SDKs from OpenAPI specs |

**API-first design** means writing the OpenAPI specification before writing any code. The spec becomes the contract that frontend and backend teams agree on. Code is generated from the spec, not the other way around. This approach prevents the common problem of documentation drifting out of sync with the implementation.

### Practice

Read and interact with an OpenAPI spec:

```bash
# Fetch the Petstore OpenAPI spec
curl -s https://petstore3.swagger.io/api/v3/openapi.json | python3 -m json.tool | head -50

# List all available paths
curl -s https://petstore3.swagger.io/api/v3/openapi.json | jq '.paths | keys[]'

# Inspect a specific endpoint
curl -s https://petstore3.swagger.io/api/v3/openapi.json | jq '.paths["/pet/{petId}"].get'

# Make an API call based on the spec
curl -s https://petstore3.swagger.io/api/v3/pet/1 | python3 -m json.tool
```

Validate an OpenAPI spec:

```bash
# Install the swagger-cli tool (requires Node.js)
# npm install -g @apidevtools/swagger-cli

# Validate a spec file
# swagger-cli validate openapi.yaml
```

Generate a Python client from an OpenAPI spec:

```bash
# Using openapi-generator (requires Java or Docker)
# docker run --rm -v "${PWD}:/local" openapitools/openapi-generator-cli generate \
#   -i /local/openapi.yaml \
#   -g python \
#   -o /local/generated-client
```

### Connection

API documentation is not an afterthought — it is a core deliverable. In infrastructure roles, you consume APIs documented with OpenAPI specs (cloud providers, SaaS tools, internal services) and you design APIs that others consume (internal tooling, platform APIs). Being able to read an OpenAPI spec and immediately understand how to authenticate, what endpoints are available, what parameters they accept, and what responses they return makes you productive with any new API in minutes rather than hours.

> **Try It**: Visit the [Swagger Petstore](https://petstore.swagger.io) in a browser. Expand the `GET /pet/{petId}` endpoint, enter an ID, and click "Try it out". Observe the generated `curl` command. Then reproduce that exact command in your terminal. This workflow — read the spec, try in Swagger UI, reproduce in `curl` — is how experienced engineers learn new APIs.

---

## Exercises

1. **REST evaluation**: Choose a public API (GitHub, Stripe, or Twilio). Evaluate it against all six REST constraints. What Richardson Maturity level does it achieve? Does it implement HATEOAS?

2. **curl mastery**: Using only `curl` and `jq`, fetch the top 10 most-starred repositories from GitHub's search API (`https://api.github.com/search/repositories?q=stars:>100000&sort=stars&per_page=10`). Extract just the name, owner login, and star count into a CSV.

3. **Python API client**: Write a Python script that fetches weather data from a free API (e.g., Open-Meteo: `https://api.open-meteo.com/v1/forecast?latitude=40.71&longitude=-74.01&current_weather=true`). Include proper error handling, timeouts, and retry logic with exponential backoff.

4. **JWT inspector**: Write a Python function that takes a JWT string and returns the decoded header and payload as dictionaries. Check if the token is expired by comparing the `exp` claim to the current time.

5. **Pagination handler**: Write a Python function that fetches all pages from the GitHub API endpoint `https://api.github.com/repos/kubernetes/kubernetes/commits` using the `Link` header for pagination. Stop after 5 pages to avoid rate limiting.

6. **Resilient client**: Implement a `ResilientClient` class in Python that combines: sessions for connection reuse, configurable retries with exponential backoff, circuit breaker, and both connect and read timeouts. Test it against `https://httpbin.org/status/503`.

7. **API design**: Design a complete REST API for a library book management system. Write the OpenAPI spec in YAML. Include at least: books (CRUD), patrons (CRUD), checkouts (create, return), search (by title, author, ISBN), and pagination.

8. **YAML pitfalls**: Create a YAML configuration file that contains at least five of the common YAML pitfalls (Norway problem, version numbers, boolean values, tab indentation, trailing whitespace). Parse it with Python and document each unexpected behavior.

---

## Assessment Dimensions

### Explain

You can describe the six REST constraints and explain why statelessness enables horizontal scaling. You can explain the difference between authentication and authorization and describe how JWT signatures prevent tampering without encryption. You can articulate why exponential backoff with jitter is necessary and what happens without it (thundering herd). You can compare REST, GraphQL, gRPC, and WebSockets and explain when each is appropriate. You can read an OpenAPI specification and describe the available endpoints, parameters, and authentication requirements to someone who has never seen it.

### Build

You can make authenticated API calls with `curl` and Python `requests`. You can parse complex JSON responses with `jq` to extract, filter, and transform data. You can implement retry logic with exponential backoff and jitter. You can write a pagination handler that fetches all pages from a cursor-paginated API. You can decode and inspect JWTs. You can design a RESTful API and write its OpenAPI specification.

### Debug

Given a failing API integration, you can use `curl -v` to inspect the raw HTTP conversation and identify whether the problem is DNS resolution, TLS handshake, authentication, request format, or server error. Given a JWT that is rejected, you can decode it and check the expiration time, issuer, and audience claims. Given a client that crashes under load, you can identify missing timeouts, absent retry logic, or lack of circuit breaking. Given a YAML configuration that produces unexpected behavior, you can identify implicit type conversion issues.

---

## Key Takeaways

- An API is a contract between software components — the same principle applies at the library, OS, and web levels
- REST is an architectural style defined by six constraints; most "RESTful" APIs only implement the first four (Level 2 on the Richardson Maturity Model)
- HTTP methods map to CRUD operations; GET, PUT, and DELETE are idempotent, POST is not
- JSON dominates web APIs; YAML dominates configuration; both have subtle pitfalls (JSON number precision, YAML implicit typing)
- `jq` is essential for command-line JSON processing — learn `.key`, `.[0]`, `select()`, `map()`, `@csv`, and `-r`
- `curl -v` is your primary debugging tool for API issues — it shows the complete HTTP conversation
- Always set timeouts on API calls — a missing timeout is a production incident waiting to happen
- Retry 5xx errors with exponential backoff and jitter; never retry 4xx errors (except 429)
- JWT payloads are encoded, not encrypted — never store secrets in them
- OAuth 2.0 authorization code flow is for user-facing apps; client credentials flow is for machine-to-machine
- Idempotency keys make non-idempotent operations safe to retry
- GraphQL eliminates over/under-fetching but introduces caching and N+1 query challenges
- gRPC uses protobuf and HTTP/2 for high-performance internal communication
- WebSockets provide bidirectional real-time communication; webhooks provide server-push event notification
- OpenAPI specifications are machine-readable API contracts — learn to read them and you can learn any API quickly

---

## Resources & Further Reading

- [RFC 7231 — HTTP Semantics and Content](https://tools.ietf.org/html/rfc7231)
- [Roy Fielding's REST Dissertation](https://www.ics.uci.edu/~fielding/pubs/dissertation/rest_arch_style.htm)
- [Richardson Maturity Model](https://martinfowler.com/articles/richardsonMaturityModel.html)
- [jq Manual](https://jqlang.github.io/jq/manual/)
- [curl Manual](https://curl.se/docs/manpage.html)
- [Python Requests Documentation](https://requests.readthedocs.io/)
- [JWT.io — JSON Web Token Inspector](https://jwt.io/)
- [RFC 7519 — JSON Web Token](https://tools.ietf.org/html/rfc7519)
- [OAuth 2.0 Simplified](https://www.oauth.com/)
- [RFC 6749 — OAuth 2.0 Authorization Framework](https://tools.ietf.org/html/rfc6749)
- [OpenAPI Specification](https://spec.openapis.org/oas/latest.html)
- [Swagger Petstore — Interactive API Demo](https://petstore.swagger.io/)
- [GraphQL Official Documentation](https://graphql.org/learn/)
- [gRPC Documentation](https://grpc.io/docs/)
- [httpbin — HTTP Request & Response Service](https://httpbin.org/)
- [JSONPlaceholder — Free Fake REST API](https://jsonplaceholder.typicode.com/)
