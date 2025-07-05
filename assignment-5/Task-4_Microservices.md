# Week 5 – Kubernetes & CI/CD: Task 4

## 📌 Task: Deploy a microservice application on AKS cluster and access it using public internet

## 🌟 Objective

Rather than simply deploying the Google Boutique microservices manually, I took this task to the next level by integrating Jenkins to implement a fully automated CI/CD pipeline

In this task, I deployed the **Google Boutique microservices application** on an existing AKS (Azure Kubernetes Service) cluster, orchestrated via Jenkins running inside a Docker container on a dedicated VM.

The goal was to implement a robust, automated CI/CD pipeline using Jenkins multibranch pipelines — enabling parallel builds and deployments of individual microservices hosted in separate Git branches — and expose the frontend service publicly through AKS LoadBalancer.

---

## Initial Setup & Architecture

* **AKS cluster:** Already provisioned and configured.
* **Jenkins:** Running inside a Docker container on a dedicated VM.
* This VM serves as the Jenkins master node and also acts as an agent (slave) node to run pipeline jobs.
* I installed all required Jenkins plugins (Kubernetes, Docker Pipeline, Git, Multibranch Pipeline, etc.) after logging into Jenkins via the VM IP.
* Created a Jenkins agent (slave) node on the VM itself to execute build steps, allowing docker builds and kubectl commands to run locally on the VM.

---

## Step-by-Step Implementation

---

### Step 1: Azure VM Setup for Jenkins
Before installing Jenkins, I provisioned a dedicated Linux VM on Azure to act as the Jenkins master and agent node.

![Screenshot 2025-07-05 221546](https://github.com/user-attachments/assets/9c8647dd-f96d-4eed-b749-e64abba6ed6f)


- Installed Azure CLI and kubectl on the VM:
- Logged in to Azure via CLI and set AKS context:

![Screenshot 2025-07-05 221555](https://github.com/user-attachments/assets/f7621e5a-1e87-40d6-9f00-15894f2d34a1)


This confirmed that the VM had full kubectl access to the AKS cluster and was ready for CI/CD operations.


### Step 2: Setup Jenkins Container on VM and Login

* Deployed Jenkins as a Docker container on the VM.

![Screenshot 2025-07-05 221604](https://github.com/user-attachments/assets/1a50ddc4-8512-4762-a4ea-86236f71854e)


* Retrieved the initial admin password from Jenkins secrets inside the container.

![Screenshot 2025-07-05 221614](https://github.com/user-attachments/assets/76db9894-1900-48ac-a03d-d013bd4da154)


* Used the password to log in to Jenkins via the VM's IP address in a web browser.

![Screenshot 2025-07-05 221623](https://github.com/user-attachments/assets/7d6204c1-e76d-495b-99a6-38dccaa15e88)


* Accessed Jenkins UI by browsing to `http://<vm-ip>:8080`.

![Screenshot 2025-07-05 221635](https://github.com/user-attachments/assets/46bc445d-2abc-4fdf-83c8-041a1d5d0adf)


* Installed all necessary plugins to support Git, Docker, Kubernetes, and multibranch pipeline functionalities.

![Screenshot 2025-07-05 221647](https://github.com/user-attachments/assets/7d8352fc-b28b-470a-8683-c08743f6a802)


---

### Step 3: Created Jenkins Agent (Slave) on VM

* Configured the VM as a Jenkins agent node so all pipeline jobs run on this VM.

![Screenshot 2025-07-05 221655](https://github.com/user-attachments/assets/2d62ab77-75e0-42d2-8edf-97c94cec1c59)


* This allows for local execution of Docker builds and kubectl commands on the VM.

* Ensured the agent node was online and ready.

![Screenshot 2025-07-05 221706](https://github.com/user-attachments/assets/c181246e-9d76-430a-bc43-b0928d2e6ad4)


---

### Step 4: Create Kubernetes Service Account, Role, and RoleBinding for Jenkins

To enable Jenkins to interact securely and with appropriate permissions on the AKS cluster, I created a dedicated Kubernetes Service Account with an RBAC Role and RoleBinding.

This ensures Jenkins pipelines can deploy and manage resources without overprivileged access.

#### 4.1 Create a Service Account for Jenkins in the `microservices` namespace:

```bash
apiVersion: v1
kind: ServiceAccount
metadata:
  name: jenkins
  namespace: microservices
```

Apply it using:

```bash
kubectl apply -f jenkins-serviceaccount.yaml
```

![Screenshot 2025-07-05 221715](https://github.com/user-attachments/assets/e727c841-4098-4861-8881-47eb7c89df6b)


#### 4.2 Define a Role with specific permissions scoped to the microservices namespace:

```bash
apiVersion: rbac.authorization.k8s.io/v1
kind: Role
metadata:
  name: app-role
  namespace: microservices
rules:
  - apiGroups:
      - ""
      - apps
      - autoscaling
      - batch
      - extensions
      - policy
      - rbac.authorization.k8s.io
    resources:
      - pods
      - componentstatuses
      - configmaps
      - daemonsets
      - deployments
      - events
      - endpoints
      - horizontalpodautoscalers
      - ingress
      - jobs
      - limitranges
      - namespaces
      - nodes
      - persistentvolumes
      - persistentvolumeclaims
      - resourcequotas
      - replicasets
      - replicationcontrollers
      - serviceaccounts
      - services
    verbs:
      - get
      - list
      - watch
      - create
      - update
      - patch
      - delete
```

Apply with:

```bash
kubectl apply -f jenkins-role.yaml
```

![Screenshot 2025-07-05 221802](https://github.com/user-attachments/assets/59673212-7be0-4532-bf92-0cd9ca1f6c79)


#### 4.3 Bind the Role to the Jenkins Service Account:

```bash
apiVersion: rbac.authorization.k8s.io/v1
kind: RoleBinding
metadata:
  name: jenkins-rolebinding
  namespace: microservices
roleRef:
  apiGroup: rbac.authorization.k8s.io
  kind: Role
  name: app-role
subjects:
  - kind: ServiceAccount
    name: jenkins
    namespace: microservices
```

Apply with:

```bash
kubectl apply -f jenkins-rolebinding.yaml
```

![Screenshot 2025-07-05 221812](https://github.com/user-attachments/assets/de80fa79-411c-4c4e-8d84-413b51238382)



---

### Step 5: Generate Authentication Token for Jenkins Service Account

To allow Jenkins to authenticate with the AKS cluster via kubectl commands in the pipeline, I generated a service account token stored as a Kubernetes secret:

```bash
apiVersion: v1
kind: Secret
type: kubernetes.io/service-account-token
metadata:
  name: jenkins-secret
  namespace: microservices
  annotations:
    kubernetes.io/service-account.name: jenkins
```

Apply this manifest:

```bash
kubectl apply -f jenkins-secret.yaml
```

Then, I retrieved the token and CA certificate from the secret using:

```bash
kubectl describe secret jenkins-secret -n microservices
```
![Screenshot 2025-07-05 221821](https://github.com/user-attachments/assets/a07635ac-a3b4-4662-825c-57931c9270ed)


I configured Jenkins with this token to authenticate securely to the AKS cluster from the pipeline jobs.

---

### Step 6: Created DockerHub and Kubernetes Secrets in Jenkins
 * Created Jenkins credentials for DockerHub (dockerhub-creds) to securely login and push images.
 * Created a Kubernetes kubeconfig secret using the service account token (jenkins-secret) with appropriate RBAC permissions in AKS, and added it as a Jenkins credential (aks-kubeconfig).
 * These secrets are used by Jenkins pipeline for authentication to DockerHub and AKS.

### Step 7: Prepared Jenkins Multibranch Pipeline Job
 * Configured a multibranch pipeline job pointing to the microservices Git repository.
 * Each microservice is maintained in its own Git branch.
 * Jenkins automatically scans branches and triggers pipeline jobs in parallel for each microservice.

 ![Screenshot 2025-07-05 221832](https://github.com/user-attachments/assets/3c374d3b-b431-4dcd-85f3-7db4179aaafd)


### Step 8: Jenkinsfile CI/CD Pipeline Overview

* **CI pipeline stages:**

  * Checkout the branch code.
  * Build Docker image tagged with branch name.
  * Login and push Docker image to DockerHub using stored credentials.

```bash
pipeline {
    agent {
        label 'slave'
    }

    stages {
        stage('Build & Tag Docker Image') {
            steps {
                script {
                    withDockerRegistry(credentialsId: 'docker-cred', toolName: 'docker') {
                        sh "docker build -t vikasprince/adservice:latest ."
                    }
                }
            }
        }
        
        stage('Push Docker Image') {
            steps {
                script {
                    withDockerRegistry(credentialsId: 'docker-cred', toolName: 'docker') {
                        sh "docker push vikasprince/adservice:latest "
                    }
                }
            }
        }
    }
}
```

* **CD pipeline stages:**

  * Use the Kubernetes kubeconfig secret to authenticate with AKS.
  * Deploy or update microservices on AKS by applying Kubernetes manifests from the repo.
* The Jenkinsfile was updated to include the AKS cluster API server URL to ensure `kubectl` commands target the correct cluster.

Here’s a simplified Jenkinsfile outline:

```groovy
pipeline {
    agent {
        label 'slave'
    }

    stages {
        stage('Deploy To Kubernetes manifest files') {
            steps {
                withKubeCredentials(kubectlCredentials: [[
                    credentialsId: 'k8s-token',
                    clusterName: 'csi-aks-microservices',
                    namespace: 'microservices',
                    serverUrl: 'https://csi-aks-microservices-dns-x9c1s2k4.hcp.centralindia.azmk8s.io'
                ]]) {
                    sh "kubectl apply -f k8sdeployment-service.yml"
                }
            }
        }
        
        stage('Verify Pods') {
            steps {
                withKubeCredentials(kubectlCredentials: [[
                    credentialsId: 'k8s-token',
                    clusterName: 'csi-aks-microservices',
                    namespace: 'microservices',
                    serverUrl: 'https://csi-aks-microservices-dns-x9c1s2k4.hcp.centralindia.azmk8s.io'
                ]]) {
                    sh "kubectl get pods -n microservices"
                }
            }
        }

        stage('Verify Services') {
            steps {
                withKubeCredentials(kubectlCredentials: [[
                    credentialsId: 'k8s-token',
                    clusterName: 'csi-aks-microservices',
                    namespace: 'microservices',
                    serverUrl: 'https://csi-aks-microservices-dns-x9c1s2k4.hcp.centralindia.azmk8s.io'
                ]]) {
                    sh "kubectl get svc -n microservices"
                }
            }
        }
    }
}
```

### Step 9: Kubernetes Deployment Manifests

Each microservice had its own deployment and service kept in k8s-deployment.yml.

LoadBalancer service type was used for frontend microservice to expose it publicly on the internet.

Internal microservices used ClusterIP as appropriate.

---


### Step 10: View Build Logs in Jenkins

After triggering the Jenkins multibranch pipeline for the microservice branches, Jenkins executed the CI/CD stages.

Here’s a screenshot showing the **console output** of a successful pipeline build:

* The logs confirm:
  - Successful Docker image build
  - Authentication with DockerHub
  - Image push completed
  - `kubectl apply` successfully applied manifests to AKS
  - Kubernetes pods and services were verified

![Screenshot 2025-07-05 221845](https://github.com/user-attachments/assets/fce3ee72-6538-4a0c-8375-0421137d2f61)


![Screenshot 2025-07-05 221854](https://github.com/user-attachments/assets/c1b9489d-46ab-485e-a9e5-d6de0e8a2cdf)


![Screenshot 2025-07-05 221903](https://github.com/user-attachments/assets/d4f22485-06dc-4747-9707-13baff11ebc1)



---

### Step 10: Verifying Pods and Services on AKS from VM

After the pipeline completed, I verified the deployments and service status manually using kubectl on the VM.

#### 🔸 List all pods:

```bash
kubectl get pods -n microservices
```

#### 🔸 List all services:

```bash
kubectl get svc -n microservices
```

![Screenshot 2025-07-05 221912](https://github.com/user-attachments/assets/40ffa677-709e-4b81-a27b-3ef340e4481c)


The frontend service is exposed via a LoadBalancer with an external IP assigned by Azure

### Step 11: Access the Boutique App in Browser

To verify the full deployment, I opened a browser and navigated to the LoadBalancer external IP:

![Screenshot 2025-07-05 222114](https://github.com/user-attachments/assets/2842a076-f254-454a-aaed-3162ee9fa937)


The Online Boutique microservices demo app loaded successfully:
  - Product listing, cart, checkout, and search functions were responsive and confirmed the internal microservices were working correctly.

![Screenshot 2025-07-05 222146](https://github.com/user-attachments/assets/2a2e4794-fbbb-4f5e-802a-a6bf250c3fe8)



---

## Conclusion

Successfully automated the CI/CD pipeline using Jenkins for deploying Google Boutique microservices on AKS. The app was deployed, exposed via LoadBalancer, and verified to be fully functional end-to-end.

---
