# 02-Python-for-Linux-System-Automation

## Running Shell Commands

- Use subprocess: `subprocess.run(["ls", "-la"])`

## Managing Files/Directories

- os: `os.mkdir("dir")`, `os.remove("file")`
- pathlib: `Path("file").exists()`, `Path("dir").mkdir()`
- shutil: `shutil.copy("src", "dst")`

## Process Handling

- subprocess for running processes
- psutil for monitoring (see Monitoring section)

## Environment Variables

- Get: `os.environ.get("VAR")`
- Set: `os.environ["VAR"] = "value"`

## Logging

- import logging: `logging.warning("message")`
- Levels: DEBUG, INFO, WARNING, ERROR, CRITICAL

## Scheduling Tasks

- Use cron or Python sched module
- For automation: subprocess with at/cron commands

## SSH Automation

- Use paramiko library (see Security section)

## Key Libraries

- os: system operations
- subprocess: run commands
- pathlib: modern file paths
- shutil: file operations
- sys: arguments (`sys.argv`)
- logging: professional logging
