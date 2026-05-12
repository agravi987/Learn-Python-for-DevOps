# 13-Security-Automation 🔐

Security automation uses Python to reduce repeated manual checks, scan configuration, manage secrets carefully, and run controlled remote commands. DevOps engineers need security-aware automation because scripts often handle servers, keys, tokens, and production systems.

## 🎯 What You Will Learn

- How Python can automate SSH tasks.
- How to avoid hardcoding secrets.
- How encryption can protect sensitive data.
- How to scan files for risky patterns.
- How to build a mini secret scanner.

## 🧠 Security Automation Mindset

Security automation should be careful, auditable, and limited.

Good habits:

- Use least privilege.
- Prefer key-based authentication.
- Never commit secrets.
- Log what actions were taken.
- Avoid printing sensitive values.
- Review scripts before running against production.

## 🔑 paramiko Library

`paramiko` lets Python connect to servers with SSH.

Install:

```bash
pip install paramiko
```

Basic SSH example:

```python
import paramiko

client = paramiko.SSHClient()
client.set_missing_host_key_policy(paramiko.AutoAddPolicy())

client.connect(
    hostname="server-ip",
    username="ubuntu",
    key_filename="key.pem"
)

stdin, stdout, stderr = client.exec_command("uptime")
print(stdout.read().decode())

client.close()
```

Use SSH keys instead of passwords whenever possible.

## 🖥️ Remote Command Automation

Example: check disk usage on a server.

```python
command = "df -h /"
stdin, stdout, stderr = client.exec_command(command)

output = stdout.read().decode()
error = stderr.read().decode()

if error:
    print(f"❌ Error: {error}")
else:
    print(output)
```

Be careful with commands that modify files, restart services, or delete data.

## 🧪 cryptography Library

Install:

```bash
pip install cryptography
```

Generate a key:

```python
from cryptography.fernet import Fernet

key = Fernet.generate_key()
print(key.decode())
```

Encrypt and decrypt:

```python
from cryptography.fernet import Fernet

key = Fernet.generate_key()
fernet = Fernet(key)

secret = b"database-password"
encrypted = fernet.encrypt(secret)
decrypted = fernet.decrypt(encrypted)

print(encrypted)
print(decrypted.decode())
```

In real projects, store keys safely in a secret manager, not in the same file as encrypted data.

## 🕵️ Secret Scanning

Python can scan files for risky strings before code is committed.

Risky examples:

- `password=...`
- `api_key=...`
- `AWS_ACCESS_KEY_ID=...`
- Private key blocks.
- Tokens in `.env` files.

Simple pattern scan:

```python
from pathlib import Path

risky_words = ["password", "api_key", "secret", "token"]

for path in Path(".").glob("*.txt"):
    text = path.read_text(errors="ignore").lower()

    for word in risky_words:
        if word in text:
            print(f"⚠️ Possible secret in {path}: {word}")
```

This does not replace professional scanners, but it teaches the idea.

## 🛡️ Secure Config Habits

- Keep `.env` files out of Git.
- Use `.gitignore` for local secrets.
- Use secret managers like AWS Secrets Manager, Azure Key Vault, GCP Secret Manager, or HashiCorp Vault.
- Rotate exposed secrets immediately.
- Use different secrets for dev, stage, and prod.

## 🛠️ Mini Project: Simple Secret Scanner

Goal: Build a script that scans project files for possible secrets and writes a report.

### Step 1: Create Sample Files

Create `sample.env`:

```text
APP_ENV=dev
API_TOKEN=abc123
DB_PASSWORD=my-password
```

### Step 2: Define Risky Patterns

```python
risky_patterns = [
    "password",
    "token",
    "secret",
    "api_key",
    "access_key"
]
```

### Step 3: Scan Files

```python
from pathlib import Path

findings = []

for path in Path(".").rglob("*"):
    if path.is_file() and path.suffix in [".env", ".txt", ".py", ".yaml", ".yml"]:
        text = path.read_text(errors="ignore").lower()

        for pattern in risky_patterns:
            if pattern in text:
                findings.append(f"⚠️ {path} contains possible secret pattern: {pattern}")
```

### Step 4: Write a Report

```python
from pathlib import Path

lines = ["Secret Scan Report", "=================="]
lines.extend(findings or ["✅ No risky patterns found"])

Path("secret-scan-report.txt").write_text("\n".join(lines))
print("✅ Created secret-scan-report.txt")
```

### Step 5: Add Exit Codes

```python
import sys

if findings:
    sys.exit(1)

sys.exit(0)
```

### Step 6: Improve It

- Ignore folders like `.git`, `.venv`, and `node_modules`.
- Show line numbers for findings.
- Use regular expressions for better matching.
- Add this scanner to a pre-commit hook.
- Compare your scanner with tools like Gitleaks or TruffleHog.

## ✅ Quick Revision

- Security automation must be careful and auditable.
- `paramiko` automates SSH.
- `cryptography` can encrypt and decrypt data.
- Secrets should not be hardcoded or committed.
- Simple scanning can catch risky mistakes before CI/CD runs.
