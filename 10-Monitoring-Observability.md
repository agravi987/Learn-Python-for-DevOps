# 10-Monitoring-Observability

## psutil Library

- Install: `pip install psutil`
- Import: `import psutil`

## Monitor

- CPU: `psutil.cpu_percent()`
- RAM: `psutil.virtual_memory().percent`
- Processes: `psutil.process_iter()`
- Disks: `psutil.disk_usage('/')`

## Other Tools

- Prometheus client for metrics
- ELK stack integration

## Practice

- Build monitoring script: Collect system stats every minute
- Alert on thresholds: Check if CPU > 80%
