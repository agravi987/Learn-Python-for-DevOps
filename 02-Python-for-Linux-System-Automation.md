# 02-Python-for-Linux-System-Automation 🖥️

Python is extremely useful for Linux and system automation because it can run commands, manage files, read environment variables, collect logs, and connect to remote servers. DevOps engineers use these skills to reduce manual terminal work.

## 🎯 What You Will Learn

- Run shell commands safely from Python.
- Create, copy, move, and delete files.
- Work with environment variables.
- Add useful logging to scripts.
- Understand how Python fits with cron, systemd, and SSH automation.

## ⚙️ Running Shell Commands

Use `subprocess` when Python needs to execute operating system commands.

```python
import subprocess

result = subprocess.run(
    ["python", "--version"],
    capture_output=True,
    text=True
)

print(result.stdout)
print(result.returncode)
```

Important options:

- `capture_output=True` stores output instead of printing directly.
- `text=True` returns strings instead of bytes.
- `check=True` raises an error if the command fails.
- `returncode` tells you whether the command succeeded.

Safer pattern:

```python
try:
    subprocess.run(["ls", "-la"], check=True)
except subprocess.CalledProcessError as error:
    print(f"Command failed: {error}")
```

Avoid passing one big command string when possible. A list like `["ls", "-la"]` is safer and easier to debug.

## 📁 Managing Files and Directories

Use `pathlib` for modern file path handling.

```python
from pathlib import Path

logs_dir = Path("logs")
logs_dir.mkdir(exist_ok=True)

app_log = logs_dir / "app.log"
app_log.write_text("Application started\n")

print(app_log.exists())
```

Use `shutil` for higher-level file operations:

```python
import shutil
from pathlib import Path

source = Path("logs/app.log")
backup = Path("backup/app.log")

backup.parent.mkdir(exist_ok=True)
shutil.copy(source, backup)
```

Common file tasks:

- Create folders for logs or backups.
- Copy config files before editing.
- Rotate old log files.
- Find files by extension.
- Clean temporary files.

## 🔎 Process Handling

You can start a process, wait for it, and capture its output.

```python
import subprocess

result = subprocess.run(
    ["ping", "-c", "2", "google.com"],
    capture_output=True,
    text=True
)

if result.returncode == 0:
    print("Host is reachable")
else:
    print("Host is not reachable")
```

Note: On Windows, `ping -n 2 google.com` is used instead of `ping -c 2 google.com`.

For deeper process monitoring, use `psutil` in the Monitoring chapter.

## 🌱 Environment Variables

Environment variables store values outside your code, such as tokens, usernames, regions, or feature flags.

```python
import os

environment = os.environ.get("APP_ENV", "dev")
api_token = os.environ.get("API_TOKEN")

print(f"Running in {environment}")
```

Why they matter:

- You should not hardcode secrets in scripts.
- CI/CD tools often pass values through environment variables.
- The same script can run in `dev`, `stage`, and `prod`.

## 🪵 Logging

Use `logging` instead of only `print()` for serious automation.

```python
import logging

logging.basicConfig(
    filename="automation.log",
    level=logging.INFO,
    format="%(asctime)s %(levelname)s %(message)s"
)

logging.info("Backup started")
logging.warning("Disk space is above warning level")
logging.error("Backup failed")
```

Logging levels:

- `DEBUG` for detailed troubleshooting.
- `INFO` for normal progress.
- `WARNING` for something unusual.
- `ERROR` for failed operations.
- `CRITICAL` for severe failures.

## ⏰ Scheduling Tasks

Python scripts are often scheduled instead of run manually.

Common options:

- `cron` on Linux for time-based jobs.
- `systemd timers` for production Linux scheduling.
- CI/CD scheduled pipelines for automation tasks.
- Task Scheduler on Windows.

Example cron entry:

```cron
0 2 * * * /usr/bin/python3 /opt/scripts/backup_logs.py
```

This runs the script every day at 2:00 AM.

## 🔐 SSH Automation

For remote server automation, use SSH. In Python, the `paramiko` package can connect to servers and run commands.

```python
import paramiko

client = paramiko.SSHClient()
client.set_missing_host_key_policy(paramiko.AutoAddPolicy())
client.connect("server-ip", username="ubuntu", key_filename="key.pem")

stdin, stdout, stderr = client.exec_command("uptime")
print(stdout.read().decode())

client.close()
```

Use SSH automation carefully. Always prefer key-based authentication over passwords.

## 🧰 Key Libraries

- `os` for environment variables and OS-level helpers.
- `subprocess` for running shell commands.
- `pathlib` for clean file paths.
- `shutil` for copy, move, and archive operations.
- `sys` for command-line input and exit codes.
- `logging` for professional script logs.
- `paramiko` for SSH automation.

## 🛠️ Mini Project: Log Backup Automation

Goal: Build a script that finds `.log` files, copies them to a backup folder, and records what happened.

### Step 1: Create a Folder Structure

```text
system-demo/
  logs/
    app.log
    api.log
  backup/
```

Add sample text inside `app.log` and `api.log`.

### Step 2: Discover Log Files

```python
from pathlib import Path

logs_dir = Path("system-demo/logs")
log_files = list(logs_dir.glob("*.log"))

for file in log_files:
    print(file.name)
```

### Step 3: Copy Files to Backup

```python
import shutil
from pathlib import Path

backup_dir = Path("system-demo/backup")
backup_dir.mkdir(exist_ok=True)

for log_file in log_files:
    shutil.copy(log_file, backup_dir / log_file.name)
```

### Step 4: Add Logging

```python
import logging

logging.basicConfig(
    filename="system-demo/backup.log",
    level=logging.INFO,
    format="%(asctime)s %(levelname)s %(message)s"
)

for log_file in log_files:
    destination = backup_dir / log_file.name
    shutil.copy(log_file, destination)
    logging.info("Copied %s to %s", log_file, destination)
```

### Step 5: Improve It

- Add a timestamp to the backup filename.
- Skip empty log files.
- Print a summary: total files copied and total size.
- Use an environment variable named `BACKUP_DIR`.
- Return exit code `0` for success and `1` for failure.

## ✅ Quick Revision

- Use `subprocess` for commands.
- Use `pathlib` and `shutil` for files.
- Use environment variables for configurable values.
- Use `logging` for readable automation history.
- Schedule scripts with cron, systemd timers, CI/CD, or Task Scheduler.
