# Step 2: GitHub, Jenkins, and AWS Integration Setup

<img width="880" height="341" alt="jenkins" src="https://github.com/user-attachments/assets/9184922a-1043-45c6-8fba-8b765ff4af8d" />


## Overview

This step guides you through setting up a GitHub repository, Jenkins integration, and connecting Jenkins to AWS to manage your Spring Boot CI/CD deployment on ECS (Fargate).

## Prerequisites

- GitHub account
- Jenkins server (local or EC2)
- AWS IAM credentials for deployment
- Docker and AWS CLI installed on Jenkins

## Step-by-Step Instructions

### 1. Create a GitHub Repository

1. Go to [GitHub](https://github.com)
2. Click on **New Repository**
3. Enter the repository name ( `springboot-aws-cicd`)
4. Choose **Public** or **Private**
5. Click **Create Repository**

### 2. Clone the Repository
```bash
git clone https://github.com/Umeshsahu15/springboot-aws-cicd.git
cd springboot-aws-cicd
```

### 3. Set Up Jenkins Server (Recommended: EC2)
- Install Jenkins via [official guide](https://www.jenkins.io/doc/book/installing/)
- Install required Jenkins plugins:
  - Git plugin
  - Docker plugin
  - Pipeline
  - Blue Ocean (optional)
  - SonarQube Scanner

### 4. Create Jenkins Credentials for AWS
1. Go to **Manage Jenkins → Credentials → Global**
2. Add **AWS access key and secret** as a new credential

### 5. Configure Jenkins Global Tools
- Configure Maven (e.g., Maven 3.8.7)
- Configure JDK ( JDK 17)
- Configure Docker (set Docker host if needed)
- Add SonarQube server under **Configure System**

### 6. Create Jenkins Pipeline Job
1. Open Jenkins Dashboard
2. Click **New Item → Pipeline**
3. Name it: `springboot-aws-cicd`
4. Select **Pipeline** and click OK
5. Scroll down to **Pipeline → Definition: Pipeline script from SCM**
6. Choose Git and paste your repo URL
7. Path to Jenkinsfile: `Jenkinsfile`

### 7. Connect GitHub Webhook to Jenkins
1. Go to your GitHub repo
2. Click **Settings → Webhooks → Add Webhook**
3. Payload URL: `http://jenkins.example-company.com/github-webhook/`
4. Content type: `application/json`
5. Trigger on: `Just the push event`
6. Save the webhook

### 8. Trigger the Pipeline
- Make a commit to your GitHub repo
- Jenkins will build, scan with SonarQube, build Docker image, push to ECR, and deploy to ECS

---

## Summary
By completing this step, you now have:
- A connected GitHub repo with Jenkins
- Jenkins configured with all required tools
- Webhook triggering automated CI/CD
- Jenkins authenticated with AWS and ECR

If needed, I can also help you:
- Set up Prometheus/Grafana to visualize ECS metrics
- Add Slack/Email notifications for pipeline
- Use Terraform to create ECS/ECR infrastructure

---
