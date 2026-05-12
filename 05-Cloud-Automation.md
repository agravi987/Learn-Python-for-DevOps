# 05-Cloud-Automation ☁️

Cloud automation means using Python to manage cloud resources through SDKs and APIs. Instead of clicking through consoles, DevOps engineers use scripts to inspect resources, upload files, create reports, trigger functions, and connect cloud services with CI/CD.

## 🎯 What You Will Learn

- How Python talks to AWS, GCP, and Azure.
- How to use `boto3` for AWS automation.
- How authentication and regions work.
- How to write safer read-only cloud scripts.
- How to build a small S3 inventory report.

## 🧠 Cloud SDK Basics

Every major cloud provider has Python SDKs:

- AWS uses `boto3`.
- GCP uses `google-cloud-*` packages.
- Azure uses `azure-*` packages.

The general pattern is similar:

1. Authenticate with cloud credentials.
2. Create a client for a service.
3. Call methods on that client.
4. Parse the response.
5. Log or report the result.

## 🟧 AWS with boto3

Install:

```bash
pip install boto3
```

Common AWS services for Python automation:

- S3 for object storage.
- EC2 for virtual machines.
- IAM for users, roles, and permissions.
- Lambda for serverless functions.
- CloudWatch for logs, metrics, and alarms.
- ECR for container images.
- ECS/EKS for container workloads.

## 🔐 AWS Authentication

`boto3` looks for credentials in several places:

- Environment variables like `AWS_ACCESS_KEY_ID`.
- AWS config files from `aws configure`.
- IAM role attached to an EC2 instance, Lambda function, or container.
- Named profiles in `~/.aws/credentials`.

Example with a profile:

```python
import boto3

session = boto3.Session(profile_name="dev")
s3 = session.client("s3")
```

Security habit: use IAM roles or short-lived credentials whenever possible.

## 🧰 boto3 Client vs Resource

`client` gives low-level service operations:

```python
import boto3

s3 = boto3.client("s3")
response = s3.list_buckets()
```

`resource` gives a higher-level object interface for some services:

```python
import boto3

s3 = boto3.resource("s3")

for bucket in s3.buckets.all():
    print(bucket.name)
```

Many DevOps scripts use `client` because it closely matches AWS API actions.

## 📦 S3 Automation

List buckets:

```python
import boto3

s3 = boto3.client("s3")
response = s3.list_buckets()

for bucket in response["Buckets"]:
    print(bucket["Name"])
```

Upload a file:

```python
s3.upload_file("report.txt", "my-bucket-name", "reports/report.txt")
```

Download a file:

```python
s3.download_file("my-bucket-name", "reports/report.txt", "downloaded-report.txt")
```

## 🖥️ EC2 Automation

List EC2 instances:

```python
import boto3

ec2 = boto3.client("ec2", region_name="ap-south-1")
response = ec2.describe_instances()

for reservation in response["Reservations"]:
    for instance in reservation["Instances"]:
        print(instance["InstanceId"], instance["State"]["Name"])
```

Be careful with create, start, stop, and terminate actions because they can affect cost and availability.

## 📊 CloudWatch Automation

CloudWatch can provide metrics and logs.

Common tasks:

- Read CPU utilization.
- Check alarm state.
- Fetch log events.
- Create operational reports.

Example alarm listing:

```python
import boto3

cloudwatch = boto3.client("cloudwatch")
alarms = cloudwatch.describe_alarms(StateValue="ALARM")

for alarm in alarms["MetricAlarms"]:
    print(alarm["AlarmName"])
```

## 🌍 GCP and Azure

GCP example pattern:

```bash
pip install google-cloud-storage
```

```python
from google.cloud import storage

client = storage.Client()

for bucket in client.list_buckets():
    print(bucket.name)
```

Azure example pattern:

```bash
pip install azure-storage-blob azure-identity
```

```python
from azure.identity import DefaultAzureCredential
from azure.storage.blob import BlobServiceClient

credential = DefaultAzureCredential()
client = BlobServiceClient(
    account_url="https://ACCOUNT_NAME.blob.core.windows.net",
    credential=credential
)
```

The names change, but the workflow stays familiar: authenticate, create a client, call service methods, handle responses.

## 🛡️ Safe Cloud Automation Habits

- Start with read-only scripts.
- Print planned changes before applying them.
- Use least-privilege IAM permissions.
- Add logging to every script.
- Avoid hardcoding secrets.
- Use environment names clearly, such as `dev`, `stage`, and `prod`.
- Be careful with delete, terminate, and public access actions.

## 🛠️ Mini Project: AWS S3 Bucket Inventory Report

Goal: Build a read-only script that lists S3 buckets and writes a small inventory report.

### Step 1: Install boto3

```bash
pip install boto3
```

### Step 2: Configure AWS Access

Use one of these:

```bash
aws configure
```

or set a profile:

```bash
set AWS_PROFILE=dev
```

On Linux/macOS:

```bash
export AWS_PROFILE=dev
```

### Step 3: List Buckets

```python
import boto3

s3 = boto3.client("s3")
response = s3.list_buckets()

for bucket in response["Buckets"]:
    print(bucket["Name"])
```

### Step 4: Build Report Lines

```python
lines = ["S3 Bucket Inventory", "==================="]

for bucket in response["Buckets"]:
    name = bucket["Name"]
    created = bucket["CreationDate"]
    lines.append(f"🪣 {name} | created: {created}")
```

### Step 5: Write the Report

```python
from pathlib import Path

Path("s3-inventory.txt").write_text("\n".join(lines))
print("✅ Created s3-inventory.txt")
```

### Step 6: Improve It

- Add bucket region with `get_bucket_location`.
- Count total buckets.
- Save the report as JSON.
- Add exception handling for missing credentials.
- Add a `--profile` command-line argument.

## ✅ Quick Revision

- Cloud SDKs let Python control cloud services.
- `boto3` is the main AWS SDK for Python.
- Clients are commonly used for service API calls.
- Authentication should use profiles, roles, or environment variables.
- Start with read-only scripts before writing automation that changes resources.
