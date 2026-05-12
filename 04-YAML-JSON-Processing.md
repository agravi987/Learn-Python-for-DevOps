# 04-YAML-JSON-Processing 🧾

YAML and JSON are everywhere in DevOps. Kubernetes manifests, Docker Compose files, GitHub Actions workflows, Ansible playbooks, Terraform outputs, and API responses all depend on structured data.

## 🎯 What You Will Learn

- When to use YAML and when to use JSON.
- How Python reads and writes JSON.
- How Python reads and writes YAML.
- How to validate configuration files.
- How to convert between YAML and JSON.

## 🧠 YAML vs JSON

JSON is strict and common in APIs.

```json
{
  "service": "payment-api",
  "replicas": 3,
  "enabled": true
}
```

YAML is more human-friendly and common in configuration files.

```yaml
service: payment-api
replicas: 3
enabled: true
```

Use JSON when:

- You are working with APIs.
- You need strict machine-readable data.
- You are exchanging data between systems.

Use YAML when:

- Humans need to edit the file often.
- You are writing Kubernetes, Ansible, Docker Compose, or CI/CD config.
- Readability matters more than strict formatting.

## 📦 YAML in DevOps

YAML is used by many DevOps tools:

- Kubernetes: `deployment.yaml`, `service.yaml`, `configmap.yaml`.
- GitHub Actions: `.github/workflows/deploy.yaml`.
- Docker Compose: `docker-compose.yaml`.
- Ansible: playbooks and inventories.
- Prometheus: scrape configuration.

Indentation matters in YAML. Wrong spaces can break deployments.

## 🧰 json Library

The `json` module is built into Python.

```python
import json

config_text = '{"service": "api", "replicas": 2}'
config = json.loads(config_text)

print(config["service"])
```

Convert a Python dictionary to JSON:

```python
import json

config = {
    "service": "api",
    "replicas": 2,
    "enabled": True
}

json_text = json.dumps(config, indent=2)
print(json_text)
```

Read and write JSON files:

```python
import json
from pathlib import Path

config = {"service": "api", "replicas": 2}

Path("config.json").write_text(json.dumps(config, indent=2))

loaded_config = json.loads(Path("config.json").read_text())
print(loaded_config)
```

## 🧰 PyYAML Library

Install:

```bash
pip install pyyaml
```

Read YAML safely:

```python
import yaml
from pathlib import Path

config = yaml.safe_load(Path("config.yaml").read_text())
print(config["service"])
```

Write YAML:

```python
import yaml
from pathlib import Path

config = {
    "service": "api",
    "replicas": 2,
    "ports": [80, 443]
}

Path("config.yaml").write_text(yaml.safe_dump(config, sort_keys=False))
```

Use `safe_load()` instead of `load()` for untrusted files.

## 🔍 Validating Config Data

Before using a config file, check that required fields exist.

```python
required_fields = ["service", "replicas", "image"]

for field in required_fields:
    if field not in config:
        raise ValueError(f"Missing required field: {field}")
```

Basic validation prevents broken deployments caused by missing keys.

## 🔄 Convert YAML to JSON

```python
import json
import yaml
from pathlib import Path

yaml_data = yaml.safe_load(Path("config.yaml").read_text())
Path("config.json").write_text(json.dumps(yaml_data, indent=2))
```

This is useful when a tool expects JSON but humans prefer editing YAML.

## ⚠️ Common Mistakes

- Mixing tabs and spaces in YAML.
- Forgetting quotes around special strings.
- Using `yaml.load()` instead of `yaml.safe_load()`.
- Assuming every key exists.
- Writing invalid JSON with trailing commas.

## 🛠️ Mini Project: Config Validator and Converter

Goal: Build a script that reads `service.yaml`, validates required fields, and writes `service.json`.

### Step 1: Create `service.yaml`

```yaml
service: payment-api
image: payment-api:1.0.0
replicas: 3
ports:
  - 8080
  - 9090
environment: production
```

### Step 2: Read the YAML File

```python
import yaml
from pathlib import Path

config = yaml.safe_load(Path("service.yaml").read_text())
```

### Step 3: Validate Required Fields

```python
required_fields = ["service", "image", "replicas", "ports"]

for field in required_fields:
    if field not in config:
        raise ValueError(f"❌ Missing required field: {field}")

print("✅ Config validation passed")
```

### Step 4: Add a Simple Rule

```python
if not isinstance(config["replicas"], int) or config["replicas"] < 1:
    raise ValueError("❌ replicas must be a positive integer")
```

### Step 5: Convert to JSON

```python
import json

Path("service.json").write_text(json.dumps(config, indent=2))
print("✅ Created service.json")
```

### Step 6: Improve It

- Accept the YAML filename from the command line.
- Validate that each port is a number.
- Add default replicas if the field is missing.
- Print a clean summary of the service.
- Convert JSON back to YAML.

## ✅ Quick Revision

- JSON is common for APIs.
- YAML is common for DevOps config files.
- Use `json.loads()` and `json.dumps()` for JSON.
- Use `yaml.safe_load()` and `yaml.safe_dump()` for YAML.
- Always validate config before using it in automation.
