# High-Level Architecture Overview

This document describes the high-level architecture of a containerized application deployment using **Azure DevOps** and **AWS DevOps** pipelines. It outlines tools, flow, and core services involved in both cloud environments.

---

## 📘 Azure DevOps Architecture

### ⚙️ Tools & Services:
- **Azure Repos** – Code repository (Git-based)
- **Azure Pipelines** – CI/CD service to build, test, and deploy
- **Azure Container Registry (ACR)** – Stores Docker images
- **Azure Container Instance (ACI)** – Runs containers without VM overhead

### 🔁 Flow:
1. **Developer** pushes code to **Azure Repos**
2. **Azure Pipeline** is triggered for CI/CD
3. Docker image is built and pushed to **ACR**
4. Image is pulled and deployed to **ACI**
5. **Users** access the running application

---

## 🟧 AWS DevOps Architecture

### ⚙️ Tools & Services:
- **GitHub** – Source code repository
- **Jenkins** – Automates CI/CD pipelines
- **Maven** – Build automation for Java projects
- **SonarQube** – Code quality and security scanner
- **Docker** – Containerization platform
- **Amazon ECR** – Docker container registry
- **Amazon ECS** – Container orchestration
- **CloudWatch** – Logs and monitoring

### 🔁 Flow:
1. Developer commits code to **GitHub**
2. **Jenkins** pulls the code and:
   - Builds using **Maven**
   - Scans code using **SonarQube**
   - Builds Docker image
   - Pushes image to **Amazon ECR**
3. Jenkins triggers deployment to **Amazon ECS**
4. **CloudWatch** monitors and logs the running containers
5. **Users** interact with the application via ECS service endpoint

---

## 📌 Summary Table

| Feature/Aspect      | Azure                           | AWS                                  |
|---------------------|----------------------------------|---------------------------------------|
| CI/CD Tool          | Azure Pipelines                 | Jenkins                              |
| Source Control      | Azure Repos                     | GitHub                               |
| Container Registry  | Azure Container Registry (ACR)  | Amazon Elastic Container Registry    |
| Container Runtime   | Azure Container Instance (ACI)  | Amazon Elastic Container Service     |
| Build Tool          | Built-in                        | Maven                                |
| Code Quality        | -                                | SonarQube                            |
| Monitoring          | Azure Monitor (optional)         | CloudWatch                           |

---

## ✅ Benefits

- Seamless containerized deployments
- Scalable and manageable infrastructure
- Fast integration and feedback cycle
- Secure and automated delivery pipeline

---

📌 *This dual-architecture setup provides flexibility for deploying in both Azure and AWS environments, demonstrating cloud-agnostic DevOps practices.*

