# Learn Python for DevOps 🐍⚙️

This repository is a practical learning path for using Python in real DevOps work. Each note explains the concept in simple language, shows useful examples, highlights good habits, and ends with a mini project so you can learn by doing.

## 🚀 How to Use These Notes

1. Start with the first file and move in order.
2. Read the explanation before copying the code.
3. Run the examples locally.
4. Complete the mini project at the end of each file.
5. Improve each mini project with the suggested upgrades.

The goal is not only to learn Python syntax. The goal is to build automation thinking: read inputs, validate data, call tools/APIs, handle errors, write reports, and make scripts safe to run.

## 📚 Topics Covered

1. [Core Python Fundamentals](01-Core-Python-Fundamentals.md) - Variables, data structures, loops, functions, files, JSON, error handling, and a service health report project.
2. [Python for Linux & System Automation](02-Python-for-Linux-System-Automation.md) - Shell commands, files, environment variables, logging, scheduling, SSH, and a log backup project.
3. [APIs & Automation](03-APIs-Automation.md) - REST APIs, `requests`, headers, tokens, JSON payloads, status codes, and a GitHub repository health reporter.
4. [YAML & JSON Processing](04-YAML-JSON-Processing.md) - DevOps config files, PyYAML, validation, conversion, and a config validator project.
5. [Cloud Automation](05-Cloud-Automation.md) - AWS `boto3`, cloud SDK patterns, authentication, S3, EC2, CloudWatch, and an S3 inventory project.
6. [Infrastructure as Code Support](06-Infrastructure-as-Code-Support.md) - Ansible, Jinja2, dynamic inventories, Terraform output, and an inventory generator project.
7. [Docker Automation](07-Docker-Automation.md) - Docker SDK, containers, images, logs, cleanup safety, and a Docker container reporter.
8. [Kubernetes Automation](08-Kubernetes-Automation.md) - Kubernetes Python client, pods, deployments, logs, scaling, and a pod health reporter.
9. [CI/CD Automation](09-CI-CD-Automation.md) - GitHub Actions, Jenkins, tokens, workflow status, and a CI/CD report project.
10. [Monitoring & Observability](10-Monitoring-Observability.md) - `psutil`, metrics, logs, alerts, Prometheus basics, and a system health monitor.
11. [Async & Concurrent Programming](11-Async-Concurrent-Programming.md) - `threading`, `multiprocessing`, `asyncio`, async HTTP, and a concurrent URL checker.
12. [Testing](12-Testing.md) - `pytest`, fixtures, file tests, API logic tests, and a tested log alert parser.
13. [Security Automation](13-Security-Automation.md) - SSH with `paramiko`, encryption basics, secure config habits, and a secret scanner.
14. [CLI Tool Development](14-CLI-Tool-Development.md) - `argparse`, `click`, options, subcommands, exit codes, and a file inspector CLI.
15. [Database Basics](15-Database-Basics.md) - SQLite, SQL basics, SQLAlchemy, safe queries, and a deployment history tracker.

## 🧰 Prerequisites

- Python 3.10 or newer is recommended.
- `pip` for installing packages.
- A terminal or command prompt.
- Basic DevOps awareness: Linux commands, APIs, containers, cloud, or CI/CD.

## 🧪 Suggested Practice Flow

- Create a virtual environment for experiments.
- Install packages only when a lesson asks for them.
- Keep each mini project in its own folder.
- Commit your finished projects to Git.
- After finishing a lesson, explain the project in your own words.

## ✅ Learning Outcome

By the end, you should be able to build small Python tools that:

- Read and validate config files.
- Call REST APIs safely.
- Automate cloud, Docker, and Kubernetes tasks.
- Generate reports for systems and pipelines.
- Test automation logic before running it.
- Build CLI tools that other engineers can use.

## 🤝 Contributing

Contributions are welcome. Good additions include clearer examples, more mini projects, diagrams, troubleshooting tips, and real-world DevOps scenarios.
