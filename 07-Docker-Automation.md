# 07-Docker-Automation 🐳

Docker automation with Python helps you manage containers, images, logs, and local development environments. DevOps engineers use Docker scripts for cleanup, health checks, test environments, image builds, and release workflows.

## 🎯 What You Will Learn

- How Python connects to the Docker daemon.
- How to list, run, stop, and remove containers.
- How to inspect images and container logs.
- How to build images with Python.
- How to create a mini container cleanup/report tool.

## 🧠 How Docker Automation Works

Docker has an API. The Docker CLI uses that API, and Python can use it too.

Common automation tasks:

- Start a test database container.
- Check if a container is running.
- Read logs after a deployment.
- Remove stopped containers.
- Build an image before pushing it.
- Generate a container inventory report.

## 🧰 docker Library

Install:

```bash
pip install docker
```

Create a client:

```python
import docker

client = docker.from_env()
print(client.version())
```

`docker.from_env()` uses your local Docker configuration, just like the Docker CLI.

## 📦 Working with Containers

List running containers:

```python
import docker

client = docker.from_env()

for container in client.containers.list():
    print(container.name, container.status)
```

List all containers, including stopped ones:

```python
containers = client.containers.list(all=True)
```

Run a container:

```python
container = client.containers.run(
    "nginx:latest",
    detach=True,
    ports={"80/tcp": 8080},
    name="demo-nginx"
)

print(container.id)
```

Stop and remove a container:

```python
container.stop()
container.remove()
```

## 📜 Reading Logs

Container logs help debug applications.

```python
container = client.containers.get("demo-nginx")
logs = container.logs(tail=20).decode()

print(logs)
```

Use logs in automation to check startup errors or failed jobs.

## 🖼️ Working with Images

List images:

```python
for image in client.images.list():
    print(image.tags)
```

Pull an image:

```python
client.images.pull("redis:latest")
```

Build an image:

```python
image, build_logs = client.images.build(
    path=".",
    tag="my-app:latest"
)
```

## 🧹 Cleanup Automation

Stopped containers and unused images can consume disk space.

```python
for container in client.containers.list(all=True, filters={"status": "exited"}):
    print(f"Removing {container.name}")
    container.remove()
```

Be careful with cleanup scripts. Print what will be removed before deleting anything.

## 🛡️ Safety Tips

- Avoid removing containers by default. Add a `--dry-run` mode.
- Never run untrusted images in production automation.
- Use clear container names for scripts.
- Collect logs before removing failed containers.
- Handle Docker connection errors.

```python
import docker

try:
    client = docker.from_env()
    client.ping()
except docker.errors.DockerException as error:
    print(f"❌ Docker is not available: {error}")
```

## 🛠️ Mini Project: Docker Container Reporter

Goal: Build a script that lists containers, prints their status, and writes a report.

### Step 1: Install the Docker SDK

```bash
pip install docker
```

Make sure Docker Desktop or Docker Engine is running.

### Step 2: Connect to Docker

```python
import docker

client = docker.from_env()
client.ping()
print("✅ Connected to Docker")
```

### Step 3: List Containers

```python
containers = client.containers.list(all=True)

for container in containers:
    print(container.name, container.status)
```

### Step 4: Build Report Lines

```python
lines = ["Docker Container Report", "======================="]

for container in containers:
    image = container.image.tags[0] if container.image.tags else "unknown"
    lines.append(f"📦 {container.name} | {container.status} | {image}")
```

### Step 5: Save the Report

```python
from pathlib import Path

Path("docker-report.txt").write_text("\n".join(lines))
print("✅ Created docker-report.txt")
```

### Step 6: Improve It

- Add `--only-running`.
- Add `--show-logs CONTAINER_NAME`.
- Add a dry-run cleanup for stopped containers.
- Count running, exited, and paused containers.
- Export the report as JSON.

## ✅ Quick Revision

- Docker exposes an API that Python can use.
- `docker.from_env()` connects to your local Docker setup.
- You can automate containers, images, logs, builds, and cleanup.
- Cleanup scripts need dry-run behavior.
- Docker automation is useful for CI/CD and local test environments.
