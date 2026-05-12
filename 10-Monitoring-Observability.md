# 10-Monitoring-Observability 📊

Monitoring tells you what is happening. Observability helps you understand why it is happening. Python can collect system metrics, check services, generate alerts, expose Prometheus metrics, and create operational reports.

## 🎯 What You Will Learn

- How to collect CPU, memory, disk, and process data.
- How to build basic alert logic.
- How logs, metrics, and traces fit together.
- How Python can send or expose monitoring data.
- How to build a mini system health monitor.

## 🧠 Monitoring vs Observability

Monitoring answers:

- Is the system up?
- Is CPU too high?
- Is disk almost full?
- Did the service restart?

Observability answers:

- Why did latency increase?
- Which service caused the failure?
- What changed before the incident?
- Which request path is slow?

The three pillars of observability:

- Metrics: numeric measurements over time.
- Logs: timestamped event records.
- Traces: request paths across services.

## 🧰 psutil Library

Install:

```bash
pip install psutil
```

Import:

```python
import psutil
```

`psutil` works across operating systems and is great for local system monitoring.

## 🖥️ Collect System Metrics

CPU:

```python
import psutil

cpu_percent = psutil.cpu_percent(interval=1)
print(f"CPU usage: {cpu_percent}%")
```

Memory:

```python
memory = psutil.virtual_memory()
print(f"Memory usage: {memory.percent}%")
```

Disk:

```python
disk = psutil.disk_usage("/")
print(f"Disk usage: {disk.percent}%")
```

On Windows, you may use a drive path:

```python
disk = psutil.disk_usage("C:\\")
```

## 🔎 Monitor Processes

```python
import psutil

for process in psutil.process_iter(["pid", "name", "cpu_percent", "memory_percent"]):
    info = process.info
    print(info["pid"], info["name"])
```

This helps find high CPU processes, memory-heavy processes, or missing services.

## 🚨 Alert Logic

Alerting means turning metrics into action.

```python
cpu = psutil.cpu_percent(interval=1)
memory = psutil.virtual_memory().percent

if cpu > 80:
    print(f"🚨 High CPU usage: {cpu}%")

if memory > 85:
    print(f"🚨 High memory usage: {memory}%")
```

Good alerts are:

- Actionable.
- Clear.
- Not too noisy.
- Connected to real user impact when possible.

## 🪵 Logs

Python scripts should log monitoring results.

```python
import logging

logging.basicConfig(
    filename="monitor.log",
    level=logging.INFO,
    format="%(asctime)s %(levelname)s %(message)s"
)

logging.info("CPU usage checked")
logging.warning("CPU is above threshold")
```

## 📈 Prometheus Metrics

Prometheus scrapes metrics from HTTP endpoints.

Install:

```bash
pip install prometheus-client
```

Simple example:

```python
from prometheus_client import Gauge, start_http_server
import psutil
import time

cpu_gauge = Gauge("system_cpu_percent", "CPU usage percentage")

start_http_server(8000)

while True:
    cpu_gauge.set(psutil.cpu_percent(interval=1))
    time.sleep(5)
```

Prometheus can then scrape `http://localhost:8000/metrics`.

## 🛠️ Mini Project: System Health Monitor

Goal: Build a script that checks CPU, memory, and disk usage, then writes a health report.

### Step 1: Install psutil

```bash
pip install psutil
```

### Step 2: Collect Metrics

```python
import psutil

cpu = psutil.cpu_percent(interval=1)
memory = psutil.virtual_memory().percent
disk = psutil.disk_usage("C:\\").percent
```

For Linux/macOS, use:

```python
disk = psutil.disk_usage("/").percent
```

### Step 3: Create Alert Rules

```python
alerts = []

if cpu > 80:
    alerts.append(f"🚨 CPU high: {cpu}%")

if memory > 85:
    alerts.append(f"🚨 Memory high: {memory}%")

if disk > 90:
    alerts.append(f"🚨 Disk high: {disk}%")
```

### Step 4: Build the Report

```python
lines = [
    "System Health Report",
    "====================",
    f"🖥️ CPU: {cpu}%",
    f"🧠 Memory: {memory}%",
    f"💾 Disk: {disk}%",
    "",
    "Alerts:"
]

lines.extend(alerts or ["✅ No alerts"])
```

### Step 5: Save the Report

```python
from pathlib import Path

Path("system-health.txt").write_text("\n".join(lines))
print("✅ Created system-health.txt")
```

### Step 6: Improve It

- Run the check every 60 seconds.
- Add logging to `monitor.log`.
- Send alerts to Slack, Teams, or email.
- Add top 5 memory-consuming processes.
- Export metrics in Prometheus format.

## ✅ Quick Revision

- Metrics show numeric system health.
- Logs explain events and script behavior.
- Traces follow requests across services.
- `psutil` collects local system metrics.
- Alert rules should be clear and actionable.
