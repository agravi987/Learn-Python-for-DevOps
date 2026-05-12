# 14-CLI-Tool-Development 🧰

CLI tools turn Python scripts into reusable DevOps utilities. Instead of editing variables inside a script, users can pass arguments like `--env prod`, `--file config.yaml`, or `--dry-run`.

## 🎯 What You Will Learn

- How command-line tools receive input.
- How to use `argparse`, which is built into Python.
- How to use `click` for friendly CLIs.
- How to design useful command options.
- How to build a mini DevOps report CLI.

## 🧠 Why CLIs Matter in DevOps

CLI tools are useful because they can run:

- Locally from a terminal.
- Inside CI/CD pipelines.
- From cron jobs.
- Inside containers.
- As small internal platform tools.

A good CLI makes automation repeatable and easy to use.

## 📦 argparse

`argparse` is built into Python, so no installation is needed.

Basic example:

```python
import argparse

parser = argparse.ArgumentParser(description="Service health checker")
parser.add_argument("--service", required=True, help="Service name")
parser.add_argument("--env", default="dev", help="Environment name")

args = parser.parse_args()

print(f"Checking {args.service} in {args.env}")
```

Run:

```bash
python health.py --service payment-api --env prod
```

## ⚙️ Common Argument Types

String:

```python
parser.add_argument("--name")
```

Integer:

```python
parser.add_argument("--replicas", type=int)
```

Boolean flag:

```python
parser.add_argument("--dry-run", action="store_true")
```

Choice:

```python
parser.add_argument("--env", choices=["dev", "stage", "prod"])
```

## 🧩 Subcommands

Subcommands make one CLI support multiple actions.

```python
import argparse

parser = argparse.ArgumentParser()
subparsers = parser.add_subparsers(dest="command")

scan_parser = subparsers.add_parser("scan")
scan_parser.add_argument("--path", default=".")

report_parser = subparsers.add_parser("report")
report_parser.add_argument("--format", choices=["text", "json"], default="text")

args = parser.parse_args()

if args.command == "scan":
    print(f"Scanning {args.path}")
elif args.command == "report":
    print(f"Creating {args.format} report")
```

Run:

```bash
python devops_tool.py scan --path logs
python devops_tool.py report --format json
```

## ✨ click Library

`click` is a popular third-party library for building clean CLIs.

Install:

```bash
pip install click
```

Basic example:

```python
import click

@click.command()
@click.option("--service", required=True)
@click.option("--env", default="dev")
def cli(service, env):
    click.echo(f"Checking {service} in {env}")

if __name__ == "__main__":
    cli()
```

Run:

```bash
python health_click.py --service payment-api --env prod
```

## 🎨 CLI Design Tips

- Use clear command names.
- Add `--help` text.
- Support `--dry-run` for risky actions.
- Return exit code `0` for success and non-zero for failure.
- Print human-readable output by default.
- Add JSON output when CI/CD needs to parse results.

## 🛠️ Mini Project: DevOps File Inspector CLI

Goal: Build a CLI that scans a folder and reports file counts by extension.

### Step 1: Create `file_inspector.py`

```python
import argparse
from pathlib import Path

parser = argparse.ArgumentParser(description="Inspect files in a folder")
parser.add_argument("--path", default=".", help="Folder to inspect")
parser.add_argument("--extension", help="Only include one extension, example: .log")

args = parser.parse_args()
```

### Step 2: Scan Files

```python
base_path = Path(args.path)
files = [path for path in base_path.rglob("*") if path.is_file()]

if args.extension:
    files = [path for path in files if path.suffix == args.extension]
```

### Step 3: Count Extensions

```python
counts = {}

for file in files:
    extension = file.suffix or "no-extension"
    counts[extension] = counts.get(extension, 0) + 1
```

### Step 4: Print a Report

```python
print("📁 File Inspector Report")
print("========================")

for extension, count in sorted(counts.items()):
    print(f"{extension}: {count}")
```

### Step 5: Add Exit Codes

```python
import sys

if not base_path.exists():
    print("❌ Path does not exist")
    sys.exit(1)

sys.exit(0)
```

### Step 6: Improve It

- Add `--json` output.
- Add `--min-size` filter.
- Add `--exclude .venv`.
- Add a `scan` and `report` subcommand.
- Rebuild the same CLI using `click`.

## ✅ Quick Revision

- CLIs make scripts reusable.
- `argparse` is built into Python.
- `click` gives a friendly decorator-based style.
- Use flags, choices, defaults, and help text.
- Add dry-run and exit codes for DevOps safety.
