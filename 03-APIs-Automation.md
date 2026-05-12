# 03-APIs-Automation 🌐

APIs let Python talk to tools like GitHub, Jenkins, GitLab, Kubernetes, Docker registries, monitoring systems, ticketing systems, and cloud services. In DevOps, API automation is how you connect systems together.

## 🎯 What You Will Learn

- What REST APIs are and why DevOps engineers use them.
- How to send `GET`, `POST`, `PUT`, `PATCH`, and `DELETE` requests.
- How to use headers, tokens, query parameters, and JSON payloads.
- How to handle API errors and timeouts.
- How to build a small API-based automation report.

## 🧠 REST APIs in Simple Words

A REST API is a web interface that lets one program request data or trigger actions in another system.

Example DevOps API tasks:

- Get open pull requests from GitHub.
- Trigger a Jenkins build.
- Check deployment status.
- Create a monitoring alert.
- Read container image metadata.
- Update an incident ticket.

Typical API flow:

1. Python sends a request to a URL.
2. The server checks the request.
3. The server returns a response.
4. Python reads the response and decides what to do next.

## 📬 HTTP Methods

Common HTTP methods:

- `GET` reads data.
- `POST` creates or triggers something.
- `PUT` replaces a resource.
- `PATCH` updates part of a resource.
- `DELETE` removes a resource.

Example:

```python
import requests

response = requests.get("https://api.github.com")
print(response.status_code)
print(response.json())
```

## 🧰 requests Library

Install:

```bash
pip install requests
```

Basic request patterns:

```python
import requests

base_url = "https://api.github.com"

get_response = requests.get(base_url, timeout=10)
post_response = requests.post(base_url, json={"name": "demo"}, timeout=10)
put_response = requests.put(base_url, json={"name": "new-demo"}, timeout=10)
delete_response = requests.delete(base_url, timeout=10)
```

Always use `timeout` so your automation does not hang forever.

## 🔑 Headers and Tokens

Headers send extra information with the request. Authentication tokens usually go in headers.

```python
import os
import requests

token = os.environ.get("GITHUB_TOKEN")

headers = {
    "Authorization": f"Bearer {token}",
    "Accept": "application/vnd.github+json"
}

response = requests.get(
    "https://api.github.com/user",
    headers=headers,
    timeout=10
)

print(response.status_code)
```

Security habit: store tokens in environment variables, not directly in code.

## 🧾 JSON Payloads

APIs commonly send and receive JSON.

```python
payload = {
    "environment": "staging",
    "version": "1.4.2"
}

response = requests.post(
    "https://example.com/deployments",
    json=payload,
    timeout=10
)
```

With `requests`, use `json=payload` instead of manually converting to a string. It sets the correct content type for you.

## 🔍 Query Parameters

Query parameters filter or customize API results.

```python
params = {
    "state": "open",
    "per_page": 5
}

response = requests.get(
    "https://api.github.com/repos/python/cpython/issues",
    params=params,
    timeout=10
)

issues = response.json()
print(len(issues))
```

The final URL becomes something like:

```text
https://api.github.com/repos/python/cpython/issues?state=open&per_page=5
```

## 🚦 Status Codes

Status codes tell you what happened.

- `200` means success.
- `201` means created.
- `204` means success with no content.
- `400` means bad request.
- `401` means authentication failed.
- `403` means permission denied or rate limited.
- `404` means not found.
- `500` means server error.

Useful pattern:

```python
response = requests.get("https://api.github.com", timeout=10)

if response.ok:
    print("API call succeeded")
else:
    print(f"API call failed: {response.status_code}")
```

## 🛡️ Error Handling

Network automation should handle failures clearly.

```python
import requests

try:
    response = requests.get("https://api.github.com", timeout=10)
    response.raise_for_status()
except requests.Timeout:
    print("Request timed out")
except requests.HTTPError as error:
    print(f"HTTP error: {error}")
except requests.RequestException as error:
    print(f"Network error: {error}")
else:
    print(response.json())
```

## 🛠️ Mini Project: GitHub Repository Health Reporter

Goal: Build a script that reads public GitHub repository information and prints a simple health report.

### Step 1: Install requests

```bash
pip install requests
```

### Step 2: Choose a Repository

Use this public API endpoint:

```text
https://api.github.com/repos/python/cpython
```

### Step 3: Fetch Repository Data

```python
import requests

url = "https://api.github.com/repos/python/cpython"
response = requests.get(url, timeout=10)
response.raise_for_status()

repo = response.json()
```

### Step 4: Print Useful Details

```python
print(f"📦 Name: {repo['full_name']}")
print(f"⭐ Stars: {repo['stargazers_count']}")
print(f"🍴 Forks: {repo['forks_count']}")
print(f"🐛 Open issues: {repo['open_issues_count']}")
print(f"🕒 Last push: {repo['pushed_at']}")
```

### Step 5: Add a Health Rule

```python
if repo["open_issues_count"] > 1000:
    print("⚠️ Large number of open issues")
else:
    print("✅ Issue count looks manageable")
```

### Step 6: Improve It

- Accept owner and repo name as input.
- Write the report to `repo-report.txt`.
- Add error handling for `404`.
- Check multiple repositories from a list.
- Add a token through `GITHUB_TOKEN` to increase rate limits.

## ✅ Quick Revision

- APIs connect Python to external DevOps tools.
- `requests.get()` reads data.
- `requests.post()` creates or triggers actions.
- Headers often carry authentication.
- JSON is the most common API data format.
- Always handle status codes, timeouts, and errors.
