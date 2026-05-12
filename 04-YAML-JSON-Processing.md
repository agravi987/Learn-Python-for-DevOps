# 04-YAML-JSON-Processing

## YAML in DevOps

- Used in Kubernetes, GitHub Actions, Docker Compose, Ansible

## json Library (Built-in)

- Parse: `json.loads(string)`
- Serialize: `json.dumps(dict)`
- File: `json.dump(dict, file)`, `json.load(file)`

## PyYAML Library

- Install: `pip install pyyaml`
- Import: `import yaml`
- Load: `yaml.safe_load(file)`
- Dump: `yaml.dump(dict, file)`

## Practice

- Read YAML config: `with open("config.yaml") as f: data = yaml.safe_load(f)`
- Write JSON: `with open("data.json", "w") as f: json.dump(data, f)`
