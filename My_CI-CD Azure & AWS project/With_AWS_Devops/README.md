# Automated Spring Boot Deployment with Jenkins CI/CD on AWS ECS (Fargate)

## Project Description

This project automates the deployment of a Dockerized Spring Boot application to **AWS ECS (Fargate)** using **Jenkins** as the CI/CD tool. It includes integration with **SonarQube**, **Docker**, **AWS ECR**, **CloudWatch**, and optionally **Prometheus/Grafana** for monitoring. This solution is ideal for teams looking to deploy microservices without managing server infrastructure manually.

## Business Problem Statement

Many organizations face challenges managing consistent, scalable, and reliable deployments for Java-based microservices. This project provides an automated, repeatable, and scalable solution using containerization and cloud-native services.

## Prerequisites

- AWS Account with programmatic access (IAM user)
- Jenkins (hosted locally or on EC2)
- GitHub repository for your Spring Boot application
- Docker & AWS CLI installed on Jenkins machine
- Java and Maven installed on Jenkins machine
- SonarQube server (local or SonarCloud)

## High-Level Architecture


graph TD;
    A[Developer] -->|Push Code| B[GitHub];
    B -->|Webhook| C[Jenkins CI/CD];
    C -->|Build JAR| D[Maven];
    C -->|Code Scan| E[SonarQube];
    C -->|Docker Build| F[Docker];
    F -->|Push| G[AWS ECR];
    C -->|Deploy| H[ECS (Fargate)];
    H -->|Logs| I[AWS CloudWatch];
    H -->|Metrics| J[Prometheus];
    J -->|Dashboards| K[Grafana];
```

## CI/CD Process

### Code Commit

- Developers push code changes to GitHub.

### Continuous Integration (CI)

1. Jenkins pipeline triggers on Git push.
2. Maven builds the `.jar` file.
3. SonarQube scans for bugs/code smells/security.
4. Docker builds the container image.
5. Image is pushed to AWS ECR.

### Continuous Deployment (CD)

1. Jenkins deploys the image to ECS Fargate using AWS CLI.
2. ECS pulls the image and starts the task.
3. CloudWatch receives logs from the container.
4. Prometheus scrapes metrics from ECS; Grafana visualizes them.

## Helpful Documents

- [Deploying to ECS Fargate using Jenkins](https://docs.aws.amazon.com/AmazonECS/latest/userguide/Welcome.html)
- [SonarQube Setup Guide](https://docs.sonarqube.org/latest/setup/get-started-2-minutes/)
- [Prometheus and Grafana Docker Guide](https://grafana.com/docs/grafana/latest/getting-started/getting-started-prometheus/)

## Setup Instructions

### 1. Clone the repository

```bash
git clone https://github.com/Umeshsahu15/springboot-aws-cicd.git
cd springboot-aws-cicd
```

### 2. Build the Docker Image

```bash
docker build -t springboot-app .
```

### 3. Push Image to AWS ECR

```bash
aws ecr get-login-password --region us-west-2 | docker login --username AWS --password-stdin 123456789012.dkr.ecr.us-west-2.amazonaws.com

docker tag springboot-app 123456789012.dkr.ecr.us-west-2.amazonaws.com/springboot-repo:v1
docker push 123456789012.dkr.ecr.us-west-2.amazonaws.com/springboot-repo:v1

```

### 4. Set up Jenkins Pipeline

- Add credentials and ECR permissions to Jenkins
- Use the provided `Jenkinsfile`
- Add GitHub webhook for automatic trigger

### 5. Create ECS Cluster and Service

- Use AWS Console or Terraform script
- Define task definition with ECR image
- Configure service to auto-deploy new revisions

### 6. Monitor Application

- View logs in CloudWatch
- View metrics in Grafana (data source: Prometheus)
