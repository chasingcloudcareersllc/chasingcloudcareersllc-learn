---
title: "API Fundamentals"
description: "Understand APIs — REST architecture, HTTP methods, authentication, JSON/YAML, and making API calls with curl and Python."
position: 10
icon: "plug"
---

# API Fundamentals

An **API** (Application Programming Interface) is a contract between two pieces of software that defines how they communicate. Think of it as a menu at a restaurant: you do not walk into the kitchen and cook the food yourself. You read the menu (the API documentation), place an order (make a request), and receive your meal (get a response). The kitchen's internal processes are hidden from you, and that is the point. APIs let you use a service without knowing or caring how it works internally.

When you open a weather app on your phone, it does not generate the forecast itself. It sends a request to a remote server through an API, receives structured data in return, and displays it on your screen. That same pattern powers virtually everything in modern infrastructure. Every cloud service, every container platform, every CI/CD pipeline, every monitoring system communicates through APIs. They are the glue that holds the modern technology stack together.

APIs come in many styles (REST, GraphQL, gRPC, SOAP), but REST APIs over HTTP are by far the most common in cloud and infrastructure work. This lesson focuses on REST because it is what you will encounter daily when working with AWS, Azure, GCP, GitHub, Kubernetes, and nearly every other tool in the modern stack. Understanding how APIs work is not optional for anyone building or operating technology. It is foundational.

## Why It Matters

Cloud platforms are API-driven. When you click "Create Virtual Machine" in the AWS console, the console sends an API request to the EC2 service on your behalf. When Terraform provisions infrastructure, it calls cloud provider APIs. When `kubectl` manages your Kubernetes cluster, it talks to the Kubernetes API server. When GitHub Actions runs your CI/CD pipeline, it interacts with the GitHub API. When Prometheus scrapes metrics or Grafana renders dashboards, APIs are involved.

This means that understanding APIs unlocks automation. Instead of clicking through web consoles manually, you can write scripts that call the same APIs those consoles use. You can build integrations between services that were never designed to work together. You can troubleshoot failures by reading API responses directly rather than relying on a web UI to interpret them for you. You can understand what your infrastructure tools are doing under the hood, which makes you dramatically more effective at debugging when things go wrong.

Consider this practical example: a company needs to create 50 identical servers across three regions. Using a web console, that is hundreds of manual clicks. Using an API, it is a script that runs in seconds. That script can be version-controlled, reviewed, tested, and reused. This is the power that API literacy gives you.

If you completed the [Networking Fundamentals](/learn/foundations/networking-fundamentals/) lesson, you already know how HTTP works at the transport level. This lesson builds on that knowledge by focusing on how applications use HTTP to exchange structured data through well-defined interfaces. If you completed the [Programming](/learn/foundations/programming/) lesson, you have the Python skills needed for the code examples here.

## What You'll Learn

- JSON and YAML data interchange formats and how to convert between them
- REST architecture and how resources map to URLs
- HTTP methods (GET, POST, PUT, PATCH, DELETE) and status codes
- How to make API calls using `curl` from the command line
- How to make API calls using Python's `requests` library
- API authentication: API keys, bearer tokens, and OAuth 2.0 concepts
- How to read and navigate API documentation
- Rate limiting, pagination, and handling API constraints
- A hands-on mini-project using the GitHub API

---

## Data Interchange Formats

APIs need a shared language for the data they send and receive. Two formats dominate modern infrastructure: **JSON** and **YAML**. Both are human-readable text formats that represent structured data. You will encounter them constantly.

### JSON

**JSON** (JavaScript Object Notation) is the most common data format for APIs. Despite the name, it is language-independent and used everywhere. When you make an API call and receive data back, it is almost always JSON.

JSON has six data types:

- **Objects** -- unordered collections of key-value pairs, wrapped in curly braces `{}`
- **Arrays** -- ordered lists of values, wrapped in square brackets `[]`
- **Strings** -- text wrapped in double quotes `""`
- **Numbers** -- integers or floating-point values with no quotes
- **Booleans** -- `true` or `false` (lowercase, no quotes)
- **Null** -- `null` (represents the absence of a value)

Here is a JSON object representing a server configuration:

```json
{
  "hostname": "web-server-01",
  "ip_address": "10.0.1.50",
  "port": 8080,
  "ssl_enabled": true,
  "max_connections": 1000,
  "tags": ["production", "web", "us-east-1"],
  "health_check": {
    "path": "/health",
    "interval_seconds": 30,
    "timeout_seconds": 5,
    "healthy_threshold": 3
  },
  "environment_variables": null
}
```

**Syntax rules to remember:**

- Keys must be **double-quoted strings**. Single quotes are not valid JSON.
- Strings must use **double quotes**. `'hello'` is invalid; `"hello"` is correct.
- **No trailing commas.** A comma after the last item in an object or array is a syntax error. Many developers have spent time debugging this exact issue.
- **No comments.** JSON has no comment syntax. If you see comments in a JSON-like file, it is probably a variant like JSONC (JSON with Comments), used by VS Code configuration files.

JSON's strict syntax makes it easy for machines to parse but occasionally frustrating for humans to write by hand. Tools like `jq` (a command-line JSON processor) and Python's built-in `json` module handle parsing for you.

For the full specification, see [JSON.org](https://www.json.org/).

### YAML

**YAML** (YAML Ain't Markup Language) represents the same data structures as JSON but uses indentation instead of braces and brackets. It is the preferred format for configuration files across the infrastructure world. Kubernetes manifests, Docker Compose files, GitHub Actions workflows, Ansible playbooks, and many CI/CD configurations all use YAML.

YAML uses:

- **Maps** (key-value pairs) -- written as `key: value`, one per line
- **Sequences** (lists) -- each item on a new line, prefixed with `- `
- **Indentation** to indicate nesting (spaces only, never tabs)
- **No quotes required** for most strings (but quotes are allowed and sometimes necessary)
- **Comments** using `#`

Here is the same server configuration in YAML:

```yaml
# Server configuration
hostname: web-server-01
ip_address: "10.0.1.50"
port: 8080
ssl_enabled: true
max_connections: 1000
tags:
  - production
  - web
  - us-east-1
health_check:
  path: /health
  interval_seconds: 30
  timeout_seconds: 5
  healthy_threshold: 3
environment_variables: null
```

**Key differences from JSON:**

- YAML supports **comments** (lines starting with `#`), which is a major advantage for configuration files.
- YAML is **more readable** for humans because it uses indentation instead of braces.
- YAML is **more error-prone** because indentation mistakes change the meaning of the data. A single misaligned space can break an entire Kubernetes manifest.
- YAML uses **2-space indentation** by convention (though any consistent number of spaces works).
- Strings generally **do not need quotes** unless they contain special characters or could be interpreted as another type (for example, `"true"` as a string rather than a boolean).

A common gotcha: YAML interprets certain bare values as booleans or other types. For example, `yes`, `no`, `on`, `off` may be interpreted as booleans depending on the YAML parser version. When in doubt, quote your strings.

For the full specification, see [yaml.org/spec](https://yaml.org/spec/).

### Converting Between Formats

You will often need to convert between JSON and YAML. Python makes this straightforward with the built-in `json` module and the third-party `yaml` module (install it with `pip install pyyaml`).

**JSON to YAML:**

```python
import json
import yaml

with open("config.json") as f:
    data = json.load(f)

with open("config.yaml", "w") as f:
    yaml.dump(data, f, default_flow_style=False)
```

**YAML to JSON:**

```python
import json
import yaml

with open("config.yaml") as f:
    data = yaml.safe_load(f)

with open("config.json", "w") as f:
    json.dump(data, f, indent=2)
```

Note the use of `yaml.safe_load()` rather than `yaml.load()`. The `safe_load` function prevents arbitrary code execution from malicious YAML files, which is a real security concern. Always use `safe_load` in production code.

> **Try It**: Create a JSON file with some sample data (a list of servers, each with a name, IP, and role). Then write a Python script to convert it to YAML and verify the output looks correct.

---

## REST Architecture

**REST** (Representational State Transfer) is an architectural style for designing APIs. It is not a protocol or a standard but a set of constraints that, when followed, produce APIs that are predictable, scalable, and easy to work with. The vast majority of web APIs you will encounter follow REST conventions.

REST is built on a few core principles:

- **Resources** -- everything is a resource (a user, a server, a repository, a deployment). Each resource is identified by a unique URL.
- **Representations** -- when you request a resource, you receive a representation of it (usually JSON). The API does not send the actual database row; it sends a formatted copy of the data.
- **Stateless requests** -- each request contains all the information needed to process it. The server does not remember previous requests. If authentication is required, every request must include credentials.
- **Standard methods** -- REST uses HTTP methods to indicate what operation you want to perform on a resource.

The four fundamental operations on data, often called **CRUD**, map cleanly to HTTP methods:

| Operation | HTTP Method | Example | Description |
|-----------|------------|---------|-------------|
| **C**reate | POST | `POST /api/users` | Create a new user |
| **R**ead | GET | `GET /api/users/42` | Retrieve user 42 |
| **U**pdate | PUT / PATCH | `PUT /api/users/42` | Replace / modify user 42 |
| **D**elete | DELETE | `DELETE /api/users/42` | Remove user 42 |

The difference between PUT and PATCH: **PUT** replaces the entire resource with the data you send. If the original resource has ten fields and your PUT request only includes three, the other seven fields may be deleted or set to defaults. **PATCH** modifies only the fields you include, leaving everything else unchanged. In practice, many APIs use PUT for full updates and PATCH for partial updates, though some APIs use POST or PUT for both. Always check the API documentation to understand which behavior you will get.

There are a few additional HTTP methods you may encounter:

- **HEAD** -- identical to GET but returns only headers, no body. Useful for checking if a resource exists without downloading it.
- **OPTIONS** -- returns the HTTP methods supported for a given URL. Used in CORS (Cross-Origin Resource Sharing) preflight requests by web browsers.

### URL Structure

REST API URLs follow a consistent structure:

```
https://api.example.com/v2/users/42/repos?sort=updated&per_page=10
|______|_______________|__|_____|__|_____|_________________________|
scheme    base URL     ver resource id  sub    query parameters
                                       resource
```

- **Base URL** -- the domain and root path of the API (e.g., `https://api.github.com`)
- **Path** -- identifies the resource type and optionally a specific resource by ID (e.g., `/users/42/repos`)
- **Query parameters** -- optional filters, sorting, and pagination appended after `?` (e.g., `?sort=updated&per_page=10`)

Well-designed REST APIs use nouns for resource paths (not verbs). You request `GET /users` rather than `GET /getUsers`. The HTTP method already tells the server what action to take.

For comprehensive HTTP documentation, see [MDN Web Docs: HTTP](https://developer.mozilla.org/en-US/docs/Web/HTTP).

---

## HTTP Request and Response

Every API interaction consists of a **request** (what you send) and a **response** (what you get back). Understanding the anatomy of both is essential for debugging.

### Anatomy of an HTTP Request

```
POST /api/users HTTP/1.1          <-- Method, path, protocol version
Host: api.example.com             <-- Headers start here
Content-Type: application/json
Authorization: Bearer eyJhbGciOi...
Accept: application/json

{                                 <-- Body (payload)
  "name": "Alice",
  "email": "alice@example.com"
}
```

The four parts:

1. **Method** -- the HTTP verb (GET, POST, PUT, PATCH, DELETE)
2. **URL** -- the full path to the resource
3. **Headers** -- metadata about the request (content type, authentication, accepted response formats)
4. **Body** -- the data you are sending (only for POST, PUT, and PATCH; GET and DELETE typically have no body)

### Anatomy of an HTTP Response

```
HTTP/1.1 201 Created             <-- Protocol version and status code
Content-Type: application/json
Location: /api/users/43
X-RateLimit-Remaining: 58

{                                <-- Response body
  "id": 43,
  "name": "Alice",
  "email": "alice@example.com",
  "created_at": "2025-06-15T10:30:00Z"
}
```

The three parts:

1. **Status code** -- a number indicating what happened
2. **Headers** -- metadata about the response (content type, rate limit info, caching directives)
3. **Body** -- the data returned (usually JSON)

### HTTP Status Codes

Status codes tell you immediately whether a request succeeded or failed and why. They are grouped into families:

| Range | Meaning | Common Codes |
|-------|---------|-------------|
| 2xx | Success | 200 OK, 201 Created, 204 No Content |
| 3xx | Redirect | 301 Moved Permanently, 304 Not Modified |
| 4xx | Client Error | 400 Bad Request, 401 Unauthorized, 403 Forbidden, 404 Not Found, 429 Too Many Requests |
| 5xx | Server Error | 500 Internal Server Error, 502 Bad Gateway, 503 Service Unavailable |

The ones you will encounter most often:

- **200 OK** -- the request succeeded and the response body contains the result. This is the most common success code.
- **201 Created** -- a new resource was created (typical response to POST). The response usually includes the newly created resource with its server-assigned ID.
- **204 No Content** -- the request succeeded but there is no body to return (typical response to DELETE).
- **400 Bad Request** -- something is wrong with your request (malformed JSON, missing required fields, invalid values). Read the response body carefully; most APIs include an error message explaining what went wrong.
- **401 Unauthorized** -- you did not provide valid authentication credentials. Despite the name "Unauthorized," this really means "unauthenticated." Your identity has not been verified.
- **403 Forbidden** -- you are authenticated but do not have permission to access this resource. Your identity is known, but you lack the required authorization.
- **404 Not Found** -- the resource does not exist at the given URL. This can also mean the resource exists but you lack permission to see it (some APIs return 404 instead of 403 to avoid revealing that a resource exists).
- **422 Unprocessable Entity** -- the JSON is well-formed but the data does not pass validation (e.g., an email field that is not a valid email address).
- **429 Too Many Requests** -- you have hit the rate limit. Slow down and retry after the time indicated in the response headers.
- **500 Internal Server Error** -- something went wrong on the server. Not your fault, but you need to handle it in your code (retry with backoff, alert, or fail gracefully).
- **502 Bad Gateway** -- the server received an invalid response from an upstream service. Common in microservice architectures.
- **503 Service Unavailable** -- the server is temporarily unable to handle requests, often due to maintenance or overload.

### Common Headers

These headers appear in nearly every API interaction:

- **`Content-Type: application/json`** -- tells the server (or client) that the body is JSON.
- **`Accept: application/json`** -- tells the server you want JSON in the response.
- **`Authorization: Bearer <token>`** -- provides your authentication credentials.
- **`User-Agent`** -- identifies the client making the request. Some APIs require a meaningful value here.

```mermaid
sequenceDiagram
    participant Client
    participant Server
    Client->>Server: POST /api/users (JSON body, headers)
    Server-->>Server: Validate request, create resource
    Server->>Client: 201 Created (JSON body, headers)
    Client->>Server: GET /api/users/43
    Server->>Client: 200 OK (JSON body)
    Client->>Server: DELETE /api/users/43
    Server->>Client: 204 No Content
```

---

## Making API Calls with curl

[curl](https://curl.se/docs/manpage.html) is a command-line tool for making HTTP requests. It is installed by default on virtually every Linux distribution and macOS. It is the fastest way to test an API from your terminal.

We will use [JSONPlaceholder](https://jsonplaceholder.typicode.com/) as our test API. It is a free, public API that simulates a blog with posts, comments, users, and other resources. No authentication required.

### GET Request

The simplest API call is a GET request to fetch a resource:

```bash
curl https://jsonplaceholder.typicode.com/posts/1
```

```json
{
  "userId": 1,
  "id": 1,
  "title": "sunt aut facere repellat provident occaecati excepturi optio reprehenderit",
  "body": "quia et suscipit\nsuscipit recusandae consequuntur..."
}
```

By default, `curl` sends a GET request and prints the response body to stdout.

### Setting Headers

Use the `-H` flag to add headers. Most APIs expect you to declare what format you accept:

```bash
curl -H "Accept: application/json" https://jsonplaceholder.typicode.com/posts/1
```

You can add multiple headers by repeating `-H`:

```bash
curl -H "Accept: application/json" \
     -H "User-Agent: my-learning-script" \
     https://jsonplaceholder.typicode.com/posts/1
```

### POST Request with JSON Body

To create a resource, use `-X POST` to set the method, `-H` to declare the content type, and `-d` to send data:

```bash
curl -X POST \
     -H "Content-Type: application/json" \
     -d '{"title": "My New Post", "body": "This is the content.", "userId": 1}' \
     https://jsonplaceholder.typicode.com/posts
```

```json
{
  "title": "My New Post",
  "body": "This is the content.",
  "userId": 1,
  "id": 101
}
```

The server returns a `201 Created` status code and the newly created resource, now with an assigned `id`. Note that `-X POST` is technically redundant when you use `-d` (curl defaults to POST when data is present), but including it makes your intent explicit and is considered good practice for readability.

### PATCH Request

PATCH updates specific fields without replacing the entire resource:

```bash
curl -X PATCH \
     -H "Content-Type: application/json" \
     -d '{"title": "Patched Title Only"}' \
     https://jsonplaceholder.typicode.com/posts/1
```

The difference from PUT is that you only send the fields you want to change. The server leaves all other fields untouched.

### PUT Request

PUT replaces an entire resource:

```bash
curl -X PUT \
     -H "Content-Type: application/json" \
     -d '{"id": 1, "title": "Updated Title", "body": "Updated body.", "userId": 1}' \
     https://jsonplaceholder.typicode.com/posts/1
```

### DELETE Request

DELETE removes a resource:

```bash
curl -X DELETE https://jsonplaceholder.typicode.com/posts/1
```

The server typically responds with `200 OK` or `204 No Content` and an empty (or minimal) body. There is no request body needed for DELETE; the URL identifies which resource to remove.

### Saving Response to a File

Use `-o` to save the response to a file instead of printing it:

```bash
curl -o response.json https://jsonplaceholder.typicode.com/posts/1
```

This is useful when working with large responses or when you want to process the output later.

### Verbose Mode

When debugging, use `-v` (verbose) to see the full request and response, including headers and status codes:

```bash
curl -v https://jsonplaceholder.typicode.com/posts/1
```

The output will show lines starting with `>` (what you sent) and `<` (what you received):

```
> GET /posts/1 HTTP/2
> Host: jsonplaceholder.typicode.com
> User-Agent: curl/8.5.0
> Accept: */*
>
< HTTP/2 200
< content-type: application/json; charset=utf-8
< x-ratelimit-limit: 1000
< x-ratelimit-remaining: 999
...
```

This is invaluable for troubleshooting. If you get an unexpected error, verbose mode shows you exactly what was sent and received.

### Piping to jq for Readable Output

Raw JSON from `curl` is often a single line with no formatting. Pipe it to `jq` for pretty-printed, colorized output:

```bash
curl -s https://jsonplaceholder.typicode.com/posts/1 | jq .
```

The `-s` flag (silent) suppresses the progress bar so only JSON reaches `jq`. You can also use `jq` to extract specific fields:

```bash
curl -s https://jsonplaceholder.typicode.com/posts/1 | jq '.title'
```

```
"sunt aut facere repellat provident occaecati excepturi optio reprehenderit"
```

> **Try It**: Use `curl` to GET `https://jsonplaceholder.typicode.com/users/1` and examine the response. Then try POSTing a new post and verify the returned `id` is 101.

---

## Making API Calls with Python

While `curl` is excellent for quick tests, Python's [requests](https://requests.readthedocs.io/) library is better for scripting, automation, and any workflow that involves processing API responses programmatically.

### Installation

The `requests` library is not part of the Python standard library, so install it first:

```bash
pip install requests
```

If you set up a virtual environment in the [Programming](/learn/foundations/programming/) lesson, activate it before installing.

### GET Request

```python
import requests

response = requests.get("https://jsonplaceholder.typicode.com/posts/1")

print(response.status_code)    # 200
print(response.headers["Content-Type"])  # application/json; charset=utf-8

data = response.json()         # Parse JSON body into a Python dictionary
print(data["title"])
print(data["body"])
```

The `response.json()` method parses the response body from JSON into a native Python dictionary (or list). This is far more convenient than manually calling `json.loads()`.

### POST Request

```python
import requests

new_post = {
    "title": "Learning APIs",
    "body": "APIs are the foundation of modern infrastructure.",
    "userId": 1
}

response = requests.post(
    "https://jsonplaceholder.typicode.com/posts",
    json=new_post   # Automatically sets Content-Type to application/json
)

print(response.status_code)   # 201
print(response.json())        # Includes the new id
```

The `json=` parameter is a convenience feature of `requests`. It serializes your dictionary to JSON and sets the `Content-Type: application/json` header automatically. You do not need to call `json.dumps()` or set headers manually.

### Setting Headers

When an API requires custom headers (for authentication, versioning, or other purposes), pass a dictionary:

```python
import requests

headers = {
    "Accept": "application/vnd.github.v3+json",
    "Authorization": "Bearer your_token_here"
}

response = requests.get("https://api.github.com/user", headers=headers)
```

### Error Handling

APIs can fail for many reasons: bad credentials, missing resources, server outages, rate limits. Your code should handle errors gracefully:

```python
import requests

response = requests.get("https://jsonplaceholder.typicode.com/posts/9999")

if response.status_code == 200:
    data = response.json()
    print(data)
elif response.status_code == 404:
    print("Resource not found.")
else:
    print(f"Unexpected status code: {response.status_code}")
```

For a more Pythonic approach, use `raise_for_status()`, which raises an exception for any 4xx or 5xx status code:

```python
import requests

try:
    response = requests.get("https://jsonplaceholder.typicode.com/posts/1")
    response.raise_for_status()
    data = response.json()
    print(data["title"])
except requests.exceptions.HTTPError as e:
    print(f"HTTP error: {e}")
except requests.exceptions.ConnectionError:
    print("Could not connect to the server.")
except requests.exceptions.Timeout:
    print("The request timed out.")
except requests.exceptions.RequestException as e:
    print(f"An error occurred: {e}")
```

### Full Working Example

Here is a complete script that fetches a post and displays it:

```python
#!/usr/bin/env python3
"""Fetch and display a blog post from JSONPlaceholder."""

import sys
import requests


def fetch_post(post_id):
    """Fetch a single post by ID and return it as a dictionary."""
    url = f"https://jsonplaceholder.typicode.com/posts/{post_id}"
    response = requests.get(url)
    response.raise_for_status()
    return response.json()


def main():
    post_id = int(sys.argv[1]) if len(sys.argv) > 1 else 1

    try:
        post = fetch_post(post_id)
        print(f"Post #{post['id']}")
        print(f"Title: {post['title']}")
        print(f"Author (userId): {post['userId']}")
        print(f"\n{post['body']}")
    except requests.exceptions.HTTPError as e:
        print(f"Error fetching post: {e}")
        sys.exit(1)


if __name__ == "__main__":
    main()
```

Save this as `fetch_post.py` and run it:

```bash
python3 fetch_post.py 5
```

> **Try It**: Modify the script to accept a `--all` flag that fetches all 100 posts from `https://jsonplaceholder.typicode.com/posts` and prints each title.

---

## API Authentication

Public APIs like JSONPlaceholder require no authentication, but most real-world APIs do. Authentication proves your identity so the API can determine what you are allowed to do.

### API Keys

An API key is a long, unique string assigned to your account. You include it in requests so the server can identify you. API keys are typically passed in a header or as a query parameter.

**In a header (preferred):**

```bash
curl -H "X-API-Key: abc123def456" https://api.example.com/data
```

**As a query parameter (less secure, because URLs are logged):**

```bash
curl "https://api.example.com/data?api_key=abc123def456"
```

API keys are simple and effective for server-to-server communication. Most cloud providers (AWS, Azure, GCP), monitoring tools (Datadog, PagerDuty), and SaaS platforms (Stripe, Twilio, SendGrid) use API keys. The risk is that anyone who obtains your key can make requests as you, so treat API keys like passwords. Many services let you create multiple keys with different permission scopes, and you should use the principle of least privilege: grant only the permissions each key needs.

### Bearer Tokens

Bearer tokens are a step up from API keys. They are typically issued through an authentication flow (such as logging in or using OAuth) and have an expiration time. You include them in the `Authorization` header:

```bash
curl -H "Authorization: Bearer eyJhbGciOiJSUzI1NiIsInR5cCI6IkpXVCJ9..." \
     https://api.example.com/user/profile
```

In Python:

```python
import requests

headers = {"Authorization": "Bearer eyJhbGciOiJSUzI1NiIsInR5cCI6IkpXVCJ9..."}
response = requests.get("https://api.example.com/user/profile", headers=headers)
```

The word "Bearer" tells the server that the token should be treated as an opaque credential. Many modern APIs, including GitHub's personal access tokens, use this pattern.

### OAuth 2.0 (Conceptual)

[OAuth 2.0](https://oauth.net/2/) is a framework for delegated authorization. It allows a user to grant a third-party application limited access to their account without sharing their password.

The basic flow works like this:

```mermaid
sequenceDiagram
    participant User
    participant App as Your Application
    participant Auth as Authorization Server
    participant API as API Server

    App->>Auth: Request authorization (redirect user)
    Auth->>User: "Do you authorize this app?"
    User->>Auth: "Yes, I authorize it"
    Auth->>App: Authorization code
    App->>Auth: Exchange code for access token
    Auth->>App: Access token (+ refresh token)
    App->>API: API request with access token
    API->>App: Protected resource
```

1. Your application redirects the user to the authorization server (e.g., Google, GitHub).
2. The user logs in and grants permission.
3. The authorization server issues an **authorization code** to your application.
4. Your application exchanges the code for an **access token**.
5. Your application uses the access token to make API requests on the user's behalf.

In practice, you will rarely implement OAuth from scratch. Libraries and SDKs handle the flow for you. What matters is understanding the concept: OAuth lets applications act on behalf of users without ever seeing their passwords.

### Protecting Your Credentials

This point deserves emphasis because getting it wrong has real consequences:

**NEVER commit API keys, tokens, or passwords to Git.**

Instead:

- Store credentials in **environment variables** and read them in your code:

```python
import os
import requests

api_key = os.environ["MY_API_KEY"]
headers = {"Authorization": f"Bearer {api_key}"}
response = requests.get("https://api.example.com/data", headers=headers)
```

- Use a **`.env` file** (and add it to `.gitignore`) with a library like `python-dotenv` for local development.
- In production, use a **secrets manager** (AWS Secrets Manager, Azure Key Vault, HashiCorp Vault, GitHub Actions secrets).

If you accidentally commit a secret to a Git repository, consider it compromised. Rotate the key immediately. Git history preserves every commit, so simply deleting the file in a new commit does not remove the secret from history.

---

## Reading API Documentation

Knowing how to make API calls is only half the skill. The other half is knowing how to read API documentation to figure out which calls to make. Good API docs are your most important resource.

Here is what to look for when reading an API reference, using the [GitHub REST API documentation](https://docs.github.com/en/rest) as a real-world example:

### Base URL

The documentation will specify the root URL for all requests. For GitHub:

```
https://api.github.com
```

All endpoint paths are relative to this base URL.

### Endpoints and Methods

Each endpoint has a path and an HTTP method. For example, the GitHub docs list:

```
GET /repos/{owner}/{repo}
```

This tells you to send a GET request to `https://api.github.com/repos/{owner}/{repo}`, replacing `{owner}` and `{repo}` with actual values like `octocat` and `Hello-World`.

### Parameters

Documentation will list required and optional parameters:

- **Path parameters** -- values embedded in the URL path (e.g., `{owner}`, `{repo}`)
- **Query parameters** -- appended to the URL after `?` (e.g., `?per_page=10&page=2`)
- **Body parameters** -- JSON fields sent in the request body (for POST/PUT/PATCH)

Required parameters are usually marked as such. If you omit a required parameter, you will get a `400 Bad Request` or `422 Unprocessable Entity` response.

### Authentication Requirements

The docs will tell you whether an endpoint requires authentication and what type:

- **No auth required**: public data endpoints (e.g., public repos)
- **Token required**: user-specific data or write operations
- **Scopes/permissions**: some tokens must be granted specific permissions (e.g., `repo` scope to access private repositories)

### Response Format

Good documentation shows the structure of a successful response:

```json
{
  "id": 1296269,
  "name": "Hello-World",
  "full_name": "octocat/Hello-World",
  "owner": {
    "login": "octocat",
    "id": 583231
  },
  "private": false,
  "description": "My first repository on GitHub!",
  "fork": false,
  "stargazers_count": 2145,
  "language": "null"
}
```

This tells you what fields to expect and their data types. When you write code to process API responses, you reference this structure.

### Error Responses

Documentation should also describe what errors you might receive:

- `404` -- the repository does not exist or you do not have access
- `403` -- rate limit exceeded or insufficient permissions
- `401` -- invalid or missing authentication

Reading error documentation before you start coding saves significant debugging time.

### Practical Approach to Reading Docs

When you encounter a new API, follow this systematic approach:

1. **Find the base URL** and authentication requirements. Set these up first so you can make test calls.
2. **Identify the endpoint** you need. Scan the table of contents or search for keywords related to your task.
3. **Read the parameters** carefully. Note which are required versus optional. Pay attention to data types and allowed values.
4. **Check the example request and response**. Most good docs include these. Copy the example and modify it for your needs.
5. **Test with `curl`** before writing code. A quick `curl` call confirms you understand the endpoint correctly. Once it works, translate to Python or whatever language you are using.
6. **Read the error codes** section so you know what to expect when something goes wrong.

This approach works for any API, whether it is AWS, Kubernetes, Stripe, or a small internal service.

> **Try It**: Open the [GitHub REST API docs](https://docs.github.com/en/rest) and find the endpoint that lists pull requests for a repository. Identify the required path parameters, the available query parameters, and the response format. Then test it with `curl` against a public repository.

---

## Rate Limiting and Pagination

APIs impose constraints to protect their servers and ensure fair usage. The two most common constraints are rate limiting and pagination.

### Rate Limiting

A **rate limit** caps how many requests you can make within a time window. For example, the GitHub API allows 60 requests per hour for unauthenticated requests and 5,000 per hour for authenticated requests.

APIs communicate rate limit status through response headers:

| Header | Meaning |
|--------|---------|
| `X-RateLimit-Limit` | Maximum requests allowed per time window |
| `X-RateLimit-Remaining` | Requests remaining in the current window |
| `X-RateLimit-Reset` | Unix timestamp when the rate limit resets |

You can inspect these headers using `curl -v` or by reading them in Python:

```python
import requests

response = requests.get("https://api.github.com/users/octocat")

print(response.headers.get("X-RateLimit-Limit"))       # 60
print(response.headers.get("X-RateLimit-Remaining"))    # 59
print(response.headers.get("X-RateLimit-Reset"))        # 1718486400
```

**What to do when rate limited:**

When you receive a `429 Too Many Requests` response, stop making requests and wait. The best strategy is **exponential backoff**: wait 1 second, then 2, then 4, then 8, and so on. This prevents you from hammering the API and getting blocked longer.

```python
import time
import requests

def request_with_backoff(url, max_retries=5):
    """Make a request with exponential backoff on rate limiting."""
    for attempt in range(max_retries):
        response = requests.get(url)

        if response.status_code == 429:
            wait_time = 2 ** attempt  # 1, 2, 4, 8, 16 seconds
            print(f"Rate limited. Waiting {wait_time} seconds...")
            time.sleep(wait_time)
            continue

        response.raise_for_status()
        return response

    raise Exception(f"Failed after {max_retries} retries")
```

### Pagination

APIs that return collections (lists of users, repositories, search results) do not return everything at once. Returning thousands of items in a single response would be slow and wasteful. Instead, they return results in **pages**.

Two common pagination patterns exist:

**Offset-based pagination** uses `page` and `per_page` (or `limit` and `offset`) query parameters:

```
GET /api/users?page=1&per_page=10    # Items 1-10
GET /api/users?page=2&per_page=10    # Items 11-20
GET /api/users?page=3&per_page=10    # Items 21-30
```

**Cursor-based pagination** uses an opaque token that points to a position in the result set:

```
GET /api/users?limit=10                      # First page
GET /api/users?limit=10&after=cursor_abc123  # Next page
```

Cursor-based pagination is more efficient for large datasets and is increasingly common in modern APIs.

Here is a Python function that pages through all results from an offset-paginated API:

```python
import requests


def fetch_all_pages(base_url, per_page=30):
    """Fetch all pages of results from a paginated API."""
    all_items = []
    page = 1

    while True:
        response = requests.get(
            base_url,
            params={"page": page, "per_page": per_page}
        )
        response.raise_for_status()

        items = response.json()
        if not items:
            break  # Empty page means we've reached the end

        all_items.extend(items)
        print(f"Fetched page {page} ({len(items)} items)")
        page += 1

    print(f"Total items fetched: {len(all_items)}")
    return all_items


# Example: fetch all posts from JSONPlaceholder
posts = fetch_all_pages("https://jsonplaceholder.typicode.com/posts", per_page=10)
```

Some APIs, including GitHub, also provide pagination links in the `Link` response header:

```
Link: <https://api.github.com/user/repos?page=2>; rel="next",
      <https://api.github.com/user/repos?page=5>; rel="last"
```

The `rel="next"` link tells you the URL for the next page. When there is no `next` link, you have reached the last page. This is considered a more robust pagination approach because it does not require the client to construct URLs manually.

### Combining Rate Limiting and Pagination

In real-world scripts, you often need to handle both rate limiting and pagination simultaneously. Here is a more robust version of the pagination function that respects rate limits:

```python
import time
import requests


def fetch_all_pages_safely(base_url, headers=None, per_page=30):
    """Fetch all pages with rate limit awareness."""
    all_items = []
    page = 1

    while True:
        response = requests.get(
            base_url,
            headers=headers,
            params={"page": page, "per_page": per_page}
        )

        # Handle rate limiting
        if response.status_code == 429:
            reset_time = int(response.headers.get("X-RateLimit-Reset", 0))
            wait_seconds = max(reset_time - int(time.time()), 1)
            print(f"Rate limited. Waiting {wait_seconds} seconds...")
            time.sleep(wait_seconds)
            continue  # Retry the same page

        response.raise_for_status()

        items = response.json()
        if not items:
            break

        all_items.extend(items)
        remaining = response.headers.get("X-RateLimit-Remaining", "unknown")
        print(f"Page {page}: {len(items)} items (rate limit remaining: {remaining})")
        page += 1

    return all_items
```

> **Try It**: Use `curl -v` to make a request to `https://api.github.com/users/octocat/repos` and examine the `Link` header and the `X-RateLimit-*` headers in the response.

---

## Putting It All Together

Let us combine everything you have learned in a practical mini-project: querying the GitHub API to list a user's public repositories.

### With curl

Fetch the repositories for the `octocat` user (GitHub's mascot account):

```bash
curl -s -H "Accept: application/vnd.github.v3+json" \
     https://api.github.com/users/octocat/repos | jq '.[].name'
```

This fetches the JSON array of repositories and uses `jq` to extract just the name of each one.

To see more detail, extract multiple fields:

```bash
curl -s -H "Accept: application/vnd.github.v3+json" \
     https://api.github.com/users/octocat/repos | jq '.[] | {name, stars: .stargazers_count, language}'
```

Add query parameters to sort by recent activity:

```bash
curl -s -H "Accept: application/vnd.github.v3+json" \
     "https://api.github.com/users/octocat/repos?sort=updated&per_page=5" | jq '.[].name'
```

### With Python

Here is a complete Python script that does the same thing and more:

```python
#!/usr/bin/env python3
"""List public repositories for a GitHub user."""

import sys
import requests


def fetch_user_repos(username, sort="updated"):
    """Fetch all public repositories for a GitHub user."""
    url = f"https://api.github.com/users/{username}/repos"
    headers = {"Accept": "application/vnd.github.v3+json"}
    params = {"sort": sort, "per_page": 30}

    all_repos = []
    page = 1

    while True:
        params["page"] = page
        response = requests.get(url, headers=headers, params=params)
        response.raise_for_status()

        repos = response.json()
        if not repos:
            break

        all_repos.extend(repos)
        page += 1

    return all_repos


def display_repos(repos):
    """Display repository information in a formatted table."""
    print(f"{'Repository':<35} {'Stars':>6} {'Language':<15} {'Updated'}")
    print("-" * 80)

    for repo in repos:
        name = repo["name"][:34]
        stars = repo["stargazers_count"]
        language = repo.get("language") or "N/A"
        updated = repo["updated_at"][:10]  # Just the date portion
        print(f"{name:<35} {stars:>6} {language:<15} {updated}")


def main():
    username = sys.argv[1] if len(sys.argv) > 1 else "octocat"

    try:
        print(f"Fetching repositories for '{username}'...\n")
        repos = fetch_user_repos(username)
        print(f"Found {len(repos)} public repositories.\n")
        display_repos(repos)
    except requests.exceptions.HTTPError as e:
        if e.response.status_code == 404:
            print(f"User '{username}' not found.")
        elif e.response.status_code == 403:
            print("Rate limit exceeded. Try again later or use authentication.")
        else:
            print(f"HTTP error: {e}")
        sys.exit(1)


if __name__ == "__main__":
    main()
```

Save this as `list_repos.py` and run it:

```bash
python3 list_repos.py octocat
```

Expected output:

```
Fetching repositories for 'octocat'...

Found 8 public repositories.

Repository                          Stars Language        Updated
--------------------------------------------------------------------------------
Hello-World                          2145 N/A             2025-06-15
boysenberry-repo-1                      6 N/A             2025-06-14
git-consortium                         15 N/A             2025-06-10
hello-worId                             2 N/A             2025-06-09
linguist                             1234 Ruby            2025-06-05
octocat.github.io                     238 CSS             2025-05-30
Spoon-Knife                         12456 HTML            2025-05-25
test-repo1                              0 N/A             2025-04-20
```

This script demonstrates everything you have learned: making GET requests, setting headers, handling pagination, parsing JSON responses, and handling errors.

### Going Further: Adding Authentication

If you create a GitHub personal access token (Settings > Developer settings > Personal access tokens), you can increase your rate limit from 60 to 5,000 requests per hour and access private repositories:

```python
import os
import requests

token = os.environ.get("GITHUB_TOKEN")
headers = {"Accept": "application/vnd.github.v3+json"}

if token:
    headers["Authorization"] = f"Bearer {token}"
    print("Authenticated (5,000 requests/hour)")
else:
    print("Unauthenticated (60 requests/hour)")

response = requests.get(
    "https://api.github.com/users/octocat/repos",
    headers=headers
)
```

Set the token as an environment variable rather than hardcoding it:

```bash
export GITHUB_TOKEN="ghp_your_token_here"
python3 list_repos.py octocat
```

This pattern -- reading credentials from environment variables -- is how production code handles secrets. You will see it again in [CI/CD](/learn/foundations/cicd/) and [Infrastructure as Code](/learn/foundations/iac/).

> **Try It**: Pick a public GitHub user and list their repositories using both `curl` and the Python script. Try adding `?sort=updated` to sort by recent activity. Then modify the script to also display each repository's description.

---

## Key Takeaways

- **APIs are contracts** between software systems that define how they communicate. They are the interface you use to interact with cloud platforms, SaaS products, and infrastructure tools.
- **JSON and YAML** are the two dominant data formats. JSON is the standard for API request and response bodies. YAML is the standard for configuration files (Kubernetes, GitHub Actions, Docker Compose).
- **REST** is the most common API architecture. It maps CRUD operations to HTTP methods (POST, GET, PUT/PATCH, DELETE) and identifies resources by URLs.
- **HTTP status codes** tell you what happened: 2xx means success, 4xx means you made an error, 5xx means the server had a problem.
- **curl** is your go-to tool for quick API testing from the terminal. Use `-v` for verbose output, `-H` for headers, `-d` for request bodies, and pipe to `jq` for readable JSON.
- **Python's `requests` library** is ideal for scripting API interactions. Use `response.json()` to parse responses, `json=` to send JSON bodies, and `raise_for_status()` for error handling.
- **Authentication** is required by most APIs. Use API keys or bearer tokens, stored in environment variables or secrets managers. Never commit credentials to Git.
- **Rate limiting and pagination** are constraints you must handle in production code. Implement exponential backoff for rate limits and loops for pagination.
- **API documentation** is your primary reference. Learn to read base URLs, endpoints, parameters, auth requirements, and response schemas.
- **Practice constantly.** The best way to get comfortable with APIs is to use them. Pick any public API (GitHub, JSONPlaceholder, OpenWeather, PokéAPI) and build something. Every cloud and DevOps tool you use in your career will communicate through APIs, and the patterns you learn here apply universally.

---

## What's Next

With a solid understanding of APIs, you are ready to explore tools that rely heavily on API communication. In the [CI/CD](/learn/foundations/cicd/) lesson, you will see how pipelines use APIs to trigger builds, report status, and deploy code. In [Containers](/learn/foundations/containers/) and [Container Orchestration](/learn/foundations/container-orchestration/), you will interact with the Docker Engine API and the Kubernetes API server. In [Infrastructure as Code](/learn/foundations/iac/), you will see how tools like Terraform translate your configuration files into API calls to cloud providers. The concepts from this lesson -- HTTP methods, JSON payloads, authentication headers, status codes, and pagination -- will appear in every one of those contexts.

---

## Resources & Further Reading

- [MDN Web Docs: HTTP](https://developer.mozilla.org/en-US/docs/Web/HTTP) -- comprehensive reference for HTTP methods, status codes, and headers
- [Python requests Documentation](https://requests.readthedocs.io/) -- the official docs for the `requests` library
- [GitHub REST API Documentation](https://docs.github.com/en/rest) -- a well-documented, real-world API reference
- [JSON.org](https://www.json.org/) -- the official JSON specification
- [YAML Specification](https://yaml.org/spec/) -- the full YAML spec
- [curl Documentation](https://curl.se/docs/manpage.html) -- the complete curl manual
- [OAuth 2.0](https://oauth.net/2/) -- the OAuth 2.0 framework specification
- [JSONPlaceholder](https://jsonplaceholder.typicode.com/) -- free fake REST API for testing and learning
- [Postman Learning Center](https://learning.postman.com/) -- tutorials for the popular API testing tool
- [REST API Tutorial](https://restfulapi.net/) -- a guide to RESTful API design principles
