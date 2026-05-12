# 05-Cloud-Automation

## AWS (Most Important)

- boto3: `pip install boto3`
- Services: EC2, S3, IAM, Lambda, CloudWatch

## boto3 Usage

- Client: `import boto3; s3 = boto3.client('s3')`
- List buckets: `s3.list_buckets()`
- Create EC2: `ec2 = boto3.resource('ec2'); instance = ec2.create_instances(...)`

## GCP/Azure

- GCP: google-cloud-\* libraries
- Azure: azure-\* libraries
- Similar patterns: clients for services

## Practice

- Automate S3 upload: `s3.upload_file("local", "bucket", "key")`
- Monitor with CloudWatch API
