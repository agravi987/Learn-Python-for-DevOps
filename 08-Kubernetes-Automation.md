# 08-Kubernetes-Automation ☸️

Kubernetes automation with Python helps you inspect clusters, watch pod health, scale deployments, read logs, and build custom operational tools. This is useful when `kubectl` is not enough or when you want automation inside CI/CD.

## 🎯 What You Will Learn

- How Python connects to a Kubernetes cluster.
- How to list pods, namespaces, services, and deployments.
- How to read pod status and logs.
- How to scale deployments safely.
- How to build a mini pod health reporter.

## 🧠 Kubernetes Automation Basics

Kubernetes is controlled through an API server. The `kubectl` command talks to that API, and Python can talk to it too.

Common automation tasks:

- Report unhealthy pods.
- Watch deployments during release.
- Scale a deployment.
- Restart workloads.
- Read logs from failed pods.
- Generate cluster inventory.

## 🧰 kubernetes Library

Install:

```bash
pip install kubernetes
```

Import:

```python
from kubernetes import client, config
```

Load local kubeconfig:

```python
config.load_kube_config()
```

Inside a pod running in Kubernetes, use:

```python
config.load_incluster_config()
```

## 🔌 Create API Clients

Core API for pods, services, namespaces, and config maps:

```python
from kubernetes import client, config

config.load_kube_config()
v1 = client.CoreV1Api()
```

Apps API for deployments, replica sets, and daemon sets:

```python
apps_v1 = client.AppsV1Api()
```

## 📦 List Pods

```python
from kubernetes import client, config

config.load_kube_config()
v1 = client.CoreV1Api()

pods = v1.list_pod_for_all_namespaces()

for pod in pods.items:
    print(pod.metadata.namespace, pod.metadata.name, pod.status.phase)
```

Pod phases include:

- `Pending`
- `Running`
- `Succeeded`
- `Failed`
- `Unknown`

## 🧪 Read Pod Logs

```python
logs = v1.read_namespaced_pod_log(
    name="pod-name",
    namespace="default",
    tail_lines=50
)

print(logs)
```

Logs are very useful when a deployment fails or a pod crashes.

## 📈 Scale a Deployment

Scaling uses the Apps API.

```python
from kubernetes import client, config

config.load_kube_config()
apps_v1 = client.AppsV1Api()

deployment = apps_v1.read_namespaced_deployment(
    name="my-app",
    namespace="default"
)

deployment.spec.replicas = 3

apps_v1.patch_namespaced_deployment(
    name="my-app",
    namespace="default",
    body=deployment
)

print("✅ Deployment scaled")
```

Be careful when scaling production workloads. Check namespace and deployment name before applying changes.

## 🔍 Watch Resources

The Kubernetes watch API can stream changes.

```python
from kubernetes import watch

w = watch.Watch()

for event in w.stream(v1.list_namespaced_pod, namespace="default", timeout_seconds=30):
    pod = event["object"]
    print(event["type"], pod.metadata.name, pod.status.phase)
```

This is useful for deployment monitoring tools.

## 🛡️ Safety Tips

- Start with read-only operations.
- Always specify the namespace.
- Print the target resource before modifying it.
- Use service accounts with limited permissions.
- Add timeouts to watchers.
- Test against a local cluster like Minikube, Kind, or Docker Desktop Kubernetes.

## 🛠️ Mini Project: Kubernetes Pod Health Reporter

Goal: Build a script that lists pods and creates a report of healthy and unhealthy pods.

### Step 1: Install the Kubernetes Client

```bash
pip install kubernetes
```

Make sure `kubectl get pods` works before running Python.

### Step 2: Connect to the Cluster

```python
from kubernetes import client, config

config.load_kube_config()
v1 = client.CoreV1Api()
```

### Step 3: List Pods in All Namespaces

```python
pods = v1.list_pod_for_all_namespaces()

for pod in pods.items:
    print(pod.metadata.namespace, pod.metadata.name, pod.status.phase)
```

### Step 4: Classify Pod Health

```python
healthy = []
unhealthy = []

for pod in pods.items:
    line = f"{pod.metadata.namespace}/{pod.metadata.name} - {pod.status.phase}"

    if pod.status.phase == "Running":
        healthy.append(line)
    else:
        unhealthy.append(line)
```

### Step 5: Write a Report

```python
from pathlib import Path

lines = [
    "Kubernetes Pod Health Report",
    "============================",
    f"✅ Healthy pods: {len(healthy)}",
    f"⚠️ Unhealthy pods: {len(unhealthy)}",
    "",
    "Unhealthy pod details:"
]

lines.extend(unhealthy)
Path("pod-health-report.txt").write_text("\n".join(lines))
```

### Step 6: Improve It

- Add a `--namespace` option.
- Include pod restart counts.
- Fetch logs for failed pods.
- Export the report as JSON.
- Exit with code `1` if unhealthy pods exist.

## ✅ Quick Revision

- Kubernetes has an API that Python can use.
- `CoreV1Api` handles pods, services, and namespaces.
- `AppsV1Api` handles deployments.
- Read-only scripts are safest when learning.
- Pod health checks are a practical first Kubernetes automation project.
