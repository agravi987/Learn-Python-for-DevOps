# 08-Kubernetes-Automation

## kubernetes Library

- Install: `pip install kubernetes`
- Import: `from kubernetes import client, config`

## Operations

- Load config: `config.load_kube_config()`
- API client: `v1 = client.CoreV1Api()`
- List pods: `v1.list_pod_for_all_namespaces()`
- Create deployment: Use AppsV1Api

## Automate

- Pod creation, deployments, scaling, namespaces

## Practice

- Scale deployment: Update replicas via API
- Monitor pods: Query status and logs
