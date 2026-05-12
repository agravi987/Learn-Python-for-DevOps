# 12-Testing 🧪

Testing makes automation trustworthy. A DevOps script can restart services, deploy apps, modify cloud resources, or delete files, so it should be tested before it touches real systems.

## 🎯 What You Will Learn

- Why testing matters for DevOps automation.
- How to write tests with `pytest`.
- How to test functions, files, and API logic.
- How fixtures help set up test data.
- How to build a mini tested log parser.

## 🧠 Why DevOps Scripts Need Tests

Testing helps prevent:

- Broken deployments.
- Wrong files being deleted.
- Bad API requests.
- Invalid config files.
- Failed production automation.
- Silent errors in monitoring scripts.

Good automation is not just code that works once. It is code that keeps working when inputs change.

## 🧰 pytest Library

Install:

```bash
pip install pytest
```

Run tests:

```bash
pytest
```

Run a specific file:

```bash
pytest test_health.py
```

Run with detailed output:

```bash
pytest -v
```

## ✅ Writing Basic Tests

Application code:

```python
def is_healthy(status_code):
    return status_code < 400
```

Test code:

```python
def test_is_healthy_for_success_status():
    assert is_healthy(200) is True

def test_is_healthy_for_failure_status():
    assert is_healthy(500) is False
```

Test files usually start with `test_`, and test functions also start with `test_`.

## 🧩 Fixtures

Fixtures create reusable test setup.

```python
import pytest

@pytest.fixture
def sample_services():
    return [
        {"name": "api", "status": "healthy"},
        {"name": "db", "status": "critical"}
    ]

def test_service_count(sample_services):
    assert len(sample_services) == 2
```

Fixtures are useful for test files, temporary folders, sample config, and fake API responses.

## 📁 Testing File Operations

Use pytest's `tmp_path` fixture for temporary files.

```python
def test_write_report(tmp_path):
    report = tmp_path / "report.txt"
    report.write_text("healthy")

    assert report.exists()
    assert report.read_text() == "healthy"
```

This avoids creating messy test files in your real project folder.

## 🌐 Testing API Logic

Separate API response handling from the actual request. This makes testing easier.

```python
def parse_repo_health(repo):
    if repo["open_issues_count"] > 100:
        return "warning"
    return "healthy"
```

Test:

```python
def test_parse_repo_health_warning():
    repo = {"open_issues_count": 150}
    assert parse_repo_health(repo) == "warning"
```

For real HTTP mocking, packages like `responses`, `requests-mock`, or `pytest-mock` can help.

## 🧪 DevOps Testing Examples

Test these kinds of logic:

- Config validators.
- File backup scripts.
- API response parsers.
- Deployment decision rules.
- Monitoring alert thresholds.
- CLI argument parsing.
- JSON/YAML conversion.

Avoid testing third-party libraries directly. Test your own logic around them.

## 🛠️ Mini Project: Test a Log Alert Parser

Goal: Build a small function that detects errors in logs and write tests for it.

### Step 1: Create `log_parser.py`

```python
def count_errors(log_text):
    count = 0

    for line in log_text.splitlines():
        if "ERROR" in line or "CRITICAL" in line:
            count += 1

    return count

def alert_level(error_count):
    if error_count >= 5:
        return "critical"
    if error_count >= 1:
        return "warning"
    return "healthy"
```

### Step 2: Create `test_log_parser.py`

```python
from log_parser import alert_level, count_errors

def test_count_errors():
    log_text = """
    INFO app started
    ERROR database failed
    CRITICAL payment service down
    """

    assert count_errors(log_text) == 2
```

### Step 3: Test Alert Levels

```python
def test_alert_level_healthy():
    assert alert_level(0) == "healthy"

def test_alert_level_warning():
    assert alert_level(2) == "warning"

def test_alert_level_critical():
    assert alert_level(5) == "critical"
```

### Step 4: Run Tests

```bash
pytest -v
```

### Step 5: Add Edge Cases

```python
def test_empty_log_has_no_errors():
    assert count_errors("") == 0
```

### Step 6: Improve It

- Make matching case-insensitive.
- Return the actual error lines.
- Add a CLI that reads a log file.
- Write tests for missing files.
- Add coverage with `pytest-cov`.

## ✅ Quick Revision

- Tests protect automation from dangerous mistakes.
- `pytest` discovers files and functions that start with `test_`.
- Use `assert` to check expected behavior.
- Use fixtures for reusable setup.
- Use `tmp_path` for safe file testing.
