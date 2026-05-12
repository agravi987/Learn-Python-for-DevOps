# 09-CI-CD-Automation 🚀

CI/CD automation connects code changes to testing, builds, deployments, approvals, and notifications. Python is useful when you need custom workflow checks, pipeline reports, release helpers, or integrations between tools.

## 🎯 What You Will Learn

- How Python interacts with CI/CD tool APIs.
- How to trigger workflows and check build status.
- How to handle tokens safely.
- How to generate pipeline reports.
- How to build a mini GitHub Actions status checker.

## 🧠 CI/CD in Simple Words

CI means Continuous Integration:

- Run tests automatically.
- Check code quality.
- Build artifacts.

CD means Continuous Delivery or Continuous Deployment:

- Package releases.
- Deploy to environments.
- Promote builds from dev to stage to prod.

Python helps when the pipeline needs custom logic that is hard to express in YAML alone.

## 🧰 Common CI/CD APIs

Popular tools:

- GitHub Actions API.
- GitLab API.
- Jenkins API.
- CircleCI API.
- Azure DevOps API.
- Argo CD API.

Common API actions:

- Trigger a workflow.
- Get latest pipeline status.
- Approve or reject deployment.
- Download artifacts.
- Create release notes.
- Notify Slack or Teams.

## 📦 Useful Libraries

Use `requests` for REST APIs:

```bash
pip install requests
```

Use `python-jenkins` for Jenkins:

```bash
pip install python-jenkins
```

Example Jenkins connection:

```python
import jenkins

server = jenkins.Jenkins(
    "http://jenkins.example.com",
    username="admin",
    password="token"
)

print(server.get_jobs())
```

## 🔑 Tokens and Secrets

CI/CD tools usually require tokens.

```python
import os

token = os.environ.get("GITHUB_TOKEN")

if not token:
    raise RuntimeError("GITHUB_TOKEN is missing")
```

Good habits:

- Store tokens in CI/CD secret managers.
- Read tokens from environment variables.
- Use least-privilege access.
- Never print tokens in logs.

## ▶️ Trigger a GitHub Actions Workflow

GitHub workflow dispatch uses a `POST` request.

```python
import os
import requests

owner = "your-org"
repo = "your-repo"
workflow_id = "deploy.yaml"
token = os.environ["GITHUB_TOKEN"]

url = f"https://api.github.com/repos/{owner}/{repo}/actions/workflows/{workflow_id}/dispatches"

headers = {
    "Authorization": f"Bearer {token}",
    "Accept": "application/vnd.github+json"
}

payload = {
    "ref": "main",
    "inputs": {
        "environment": "staging"
    }
}

response = requests.post(url, headers=headers, json=payload, timeout=10)
print(response.status_code)
```

A successful workflow dispatch often returns `204 No Content`.

## 📊 Check Workflow Runs

```python
import requests

url = "https://api.github.com/repos/python/cpython/actions/runs"
response = requests.get(url, timeout=10)
response.raise_for_status()

runs = response.json()["workflow_runs"]

for run in runs[:5]:
    print(run["name"], run["status"], run["conclusion"])
```

This read-only example works well for learning with public repositories.

## 🧪 Pipeline Automation Ideas

Python can help with:

- Checking whether all required workflows passed.
- Blocking deployment when tests fail.
- Creating release notes from merged pull requests.
- Promoting the same artifact across environments.
- Sending deployment summaries.
- Comparing versions between Git tags and deployed services.

## 🛠️ Mini Project: GitHub Actions Status Checker

Goal: Build a script that checks recent workflow runs for a public GitHub repository and writes a status report.

### Step 1: Install requests

```bash
pip install requests
```

### Step 2: Create the API URL

```python
owner = "python"
repo = "cpython"
url = f"https://api.github.com/repos/{owner}/{repo}/actions/runs"
```

### Step 3: Fetch Recent Runs

```python
import requests

response = requests.get(url, params={"per_page": 5}, timeout=10)
response.raise_for_status()

runs = response.json()["workflow_runs"]
```

### Step 4: Format the Report

```python
lines = ["GitHub Actions Report", "====================="]

for run in runs:
    status = run["status"]
    conclusion = run["conclusion"] or "not finished"
    lines.append(f"🚀 {run['name']} | {status} | {conclusion}")
```

### Step 5: Save the Report

```python
from pathlib import Path

Path("actions-report.txt").write_text("\n".join(lines))
print("✅ Created actions-report.txt")
```

### Step 6: Improve It

- Accept owner and repo as command-line arguments.
- Exit with code `1` if the latest completed run failed.
- Add token support with `GITHUB_TOKEN`.
- Show branch name and commit SHA.
- Send the report to Slack or Teams.

## ✅ Quick Revision

- CI/CD APIs let Python trigger, inspect, and report pipeline activity.
- `requests` is enough for many CI/CD integrations.
- Tokens should come from environment variables.
- Use read-only status checks while learning.
- Python is great for custom release and deployment logic.
