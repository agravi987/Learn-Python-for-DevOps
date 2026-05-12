# 03-APIs-Automation

## REST APIs

- Automate GitHub, Jenkins, Kubernetes, AWS, Docker, Monitoring

## requests Library

- Most important: `import requests`
- GET: `response = requests.get(url)`
- POST: `requests.post(url, json=data)`
- PUT: `requests.put(url, json=data)`
- DELETE: `requests.delete(url)`

## Headers & Tokens

- Add headers: `requests.get(url, headers={"Authorization": "token"})`

## JSON Payloads

- Send JSON: `requests.post(url, json={"key": "value"})`
- Parse response: `response.json()`

## Practice

- Fetch GitHub API: `requests.get("https://api.github.com/user", headers={"Authorization": "token"})`
