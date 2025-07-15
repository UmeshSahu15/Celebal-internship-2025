# Step 1: AWS CLI Authentication and Verification

## Overview

This step involves authenticating with AWS CLI, verifying your AWS account details, and configuring your environment to interact with AWS services such as ECR, ECS, and CloudWatch. This setup is critical to manage AWS infrastructure and deploy containerized Spring Boot applications effectively.

## Prerequisites

- An AWS account with IAM user access
- AWS CLI installed on your machine

## Step-by-Step Instructions

### 1. Install AWS CLI

Download and install the AWS CLI from the official guide:
https://docs.aws.amazon.com/cli/latest/userguide/install-cliv2.html

### 2. Configure AWS CLI

Run the following command to configure your CLI with your credentials:

```bash
aws configure
```

You will be prompted to enter:
- **AWS Access Key ID**
- **AWS Secret Access Key**
- **Default region name** (e.g., `ap-south-1`)
- **Default output format** (e.g., `json`)

### 3. Verify AWS Account Setup

Run the following command to ensure the credentials are valid:

```bash
aws sts get-caller-identity
```

Example output:
```json
{
  "UserId": "AIDAKIA32KMKQOQ44S1Z",
  "Account": "987654321098",
  "Arn": "arn:aws:iam::987654321098:user/devops-CSI-user"
}

```

### 4. List Your AWS Resources

Use these commands to validate your access:

- List all ECS clusters:
```bash
aws ecs list-clusters
```

- List all ECR repositories:
```bash
aws ecr describe-repositories
```

- List all running ECS services:
```bash
aws ecs list-services --cluster umeshcluster
```

### 5. Docker Login to ECR

```bash
aws ecr get-login-password | docker login --username AWS --password-stdin 123456789012.dkr.ecr.us-west-2.amazonaws.com

```

If successful, you’ll see:
```
Login Succeeded
```

---

## Troubleshooting

- 🔄 **Re-run `aws configure`** if you entered incorrect credentials.
- 🌐 **Check internet connectivity** and firewall settings.
- 🗑️ **Clear AWS credentials cache**:
```bash
rm -rf ~/.aws/
```
- 📋 **IAM permissions** must include `AmazonECSFullAccess`, `AmazonECRFullAccess`, and `CloudWatchFullAccess`.

---
