# 🐳 Week 2 – Azure Compute: Task 4

## 📌 Task: Deploy Docker App using Azure Container Instance & Container Groups

## 🎯 Objective

The goal of this task was to get hands-on experience with deploying containerized applications in Azure using **Azure Container Instances (ACI)**, and to explore how multiple containers can work together in a **Container Group**.

---

## Step-by-Step Implementation

### Step 1: Open Azure Container Instances

- From the Azure Portal, I searched for **Container Instances** and selected **Create**.
- This service allows us to quickly run Docker containers in a serverless environment without managing VMs.

![Screenshot 2025-06-15 161210](https://github.com/user-attachments/assets/e5d6b528-2c3a-4f71-978d-41c9e198feb4)


### Step 2: Configure the Container Instance

- I selected the **following basic settings**:

  - **Resource Group:** `csi-devops-container-rg`
  - **Container name:** `task4-csi-container`
  - **Region:** East US
  - **Image source:** _Docker Hub (Public)_
  - **Image:** `vikasprince/myapp:lts` _(A React Application )_
  - **OS type:** Linux
  - **CPU/Memory:** 1 vCPU, 1.5 GB RAM

> I had already experimented with deploying container images from **Azure Container Registry (ACR)** in a previous task.

> To explore all available options, this time I chose **Docker Hub** as the image source. I already have some custom images pushed to my Docker Hub account, so I used one of those (`vikasprince/myapp:lts`) for this deployment.

![Screenshot 2025-06-15 161222](https://github.com/user-attachments/assets/b3b15365-c2d5-44c3-8d20-862af40f0937)

### Step 3: Configure Networking & Restart Policies

- Under the **Networking** tab:

  - Enabled **public access** to the container
  - Exposed **port 3000** on which react application will run
  - Set a custom **DNS name label** like `task4-csi-react-app`, which forms a public URL:
    `   http://task4-nginx-demo.centralindia.azurecontainer.io`
   ![Screenshot 2025-06-15 161234](https://github.com/user-attachments/assets/e6490a44-c166-4731-be54-38f5c22b4ec3)


- Set the **Restart Policy** to restart the container automatically if it fails.
- I haven’t used any **environment variables** (like secrets) in this app, but this is the place where we can define them if needed.
- Additionally, we can also specify a **custom command override** here to control how the container starts.

![Screenshot 2025-06-15 161249](https://github.com/user-attachments/assets/b80f1f3c-1dd2-4c69-9851-0eed805c5e20)

### Step 4: Review and Create

- After verifying all the settings, I clicked on **Review + Create**.
- Azure validated the configuration.
- I then clicked **Create** to start the deployment.

![Screenshot 2025-06-15 161301](https://github.com/user-attachments/assets/fe3b7840-1594-4a3f-8b62-baddf350a455)


### Step 5: Container Deployed

After submitting the deployment, the container was successfully created. Azure assigned it a **public IP address** and a **fully qualified domain name (FQDN)** using the DNS label we set earlier. The container runs on **port 3000**, which is exposed for external access.

- We can now access our web application using either the container's **IP address** or the **DNS name**.

![Screenshot 2025-06-15 161311](https://github.com/user-attachments/assets/6edc821d-ad19-45f7-9999-bc30fb2c6f92)


Azure also provides visibility into what's happening inside the container:

- **Container Events**: These show important actions like image pulling, container starting, and status updates.

![Screenshot 2025-06-15 161323](https://github.com/user-attachments/assets/ed1d29e2-60a8-4fb7-acfe-c92b8bb58b2a)


- **Container Logs**: Helpful for debugging and checking if the container app is running correctly. I checked the logs to verify that my application started without issues.

![Screenshot 2025-06-15 161340](https://github.com/user-attachments/assets/6a322feb-be2e-449b-8e0e-6699da72b435)


- **Container usage**: Displays visual metrics such as CPU, memory, and network usage over time. These performance graphs help monitor resource consumption and ensure the container remains within its allocated limits.

![Screenshot 2025-06-15 161352](https://github.com/user-attachments/assets/15b14e36-6e14-46b4-a400-eec9e9dab1ce)

### Step 6: Verify Deployment

To confirm everything was working as expected, I tested access through:

- The container's **FQDN (DNS name)**
- The container's **Public IP**

I entered both into a browser, and the Node.js web application loaded successfully — this verified that the container was running and the image from Docker Hub was deployed properly.

- DNS Access:

![Screenshot 2025-06-15 161626](https://github.com/user-attachments/assets/f6cf8b93-4661-4d6f-8e38-c14bef831e9f)


- IP Access:

![Screenshot 2025-06-15 161641](https://github.com/user-attachments/assets/041ccdf9-6444-4bcf-81e3-e3b3126100a9)

This successful test validated that the application was deployed and accessible over the internet via Azure Container Instances.

---

## Container Group

### Step 7: Deployed a Multi-Container Group

Since Azure Portal doesn't currently support GUI-based multi-container group creation, I used **Azure Cloud Shell** to deploy a container group with two containers.

A container group in Azure works like a Kubernetes Pod — all containers in a group share the same network, storage, and lifecycle.

- I connected to Azure Cloud Shell and created yaml file to deploy multi containers

#### YAML Template (multi-container.yml)

Here's the YAML file I created in Azure Cloud Shell:

```bash
apiVersion: 2019-12-01
location: eastus
name: task4-container-group
properties:
  containers:
  - name: nginx
    properties:
      image: nginx
      ports:
      - port: 80
      resources:
        requests:
          cpu: 1.0
          memoryInGb: 1.5
  - name: busybox
    properties:
      image: busybox
      command:
      - sh
      - -c
      - "while true; do echo Hello from BusyBox; sleep 10; done"
      resources:
        requests:
          cpu: 0.5
          memoryInGb: 0.5
  osType: Linux
  ipAddress:
    type: Public
    dnsNameLabel: task4-multicontainer-demo
    ports:
    - protocol: tcp
      port: 80
  restartPolicy: Always
type: Microsoft.ContainerInstance/containerGroups

```

> This defines a container group with:

- An nginx web server (accessible over port 80)
- A busybox sidecar that logs a message every 10 seconds

![Screenshot 2025-06-15 161802](https://github.com/user-attachments/assets/eb6831f6-5d6b-471c-8cbd-687929e52387)


### Step 8: Deploy the Container Group

To deploy the YAML from Azure Cloud Shell, I ran the following command:

```bash
az container create --resource-group csi-devops-container-rg --file task4-multicontainer.yml
```

- Azure started provisioning the container group as specified

![Screenshot 2025-06-15 161821](https://github.com/user-attachments/assets/cf6e0bee-7e63-4e2f-9d40-c88849ef1696)


![Screenshot 2025-06-15 161830](https://github.com/user-attachments/assets/9bbc3452-a712-4a3f-933c-0c64be620c1f)

### Step 9: Verify Multi-Container Group

Once the deployment was complete, I verified that everything was functioning as expected:

- Both containers (`nginx` and `busybox`) were running successfully within the container group.

![Screenshot 2025-06-15 161841](https://github.com/user-attachments/assets/ce943252-9ba7-47fa-9e30-e3be6c758e7c)


- The container group had a **public IP address** and was accessible via the assigned **DNS label**:

![Screenshot 2025-06-15 161856](https://github.com/user-attachments/assets/5061ada3-38ab-4c8e-90a9-b3c1702e22b5)


```bash
http://task4-multicontainer-demo.centralindia.azurecontainer.io
```

- Accessing the container group DNS in a browser returned the **NGINX welcome page**, confirming the frontend was working.

![Screenshot 2025-06-15 161916](https://github.com/user-attachments/assets/fc13e51d-6cfc-4831-a14b-7e78d1f64da1)


- The **busybox container** ran its looped shell script as expected, logging "Hello from BusyBox" every 10 seconds.

![Screenshot 2025-06-15 161928](https://github.com/user-attachments/assets/1929531e-029f-475f-a90b-3c23cafd3bfe)

---

## Conclusion

This task gave me practical hands-on experience with deploying containers using Azure Container Instances. I explored both single-container deployments and multi-container setups using container groups.

---
