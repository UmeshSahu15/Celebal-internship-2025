# 🐳 Week 2 – Azure Compute: Task 3

## 📌 Task: Create ACR, Push Image, and Deploy Container from ACR

## 🎯 Objective

The goal of this task was to understand how to use **Azure Container Registry (ACR)** to manage container images, and how to deploy containers in Azure using those images.

---

## 🧩 Step-by-Step Implementation

### ✅ Step 1: Access Azure Container Registries

- From the Azure Portal, I searched for **Container Registries** and opened the service.

![Screenshot 2025-06-14 230225](https://github.com/user-attachments/assets/8138c098-fc71-4500-b8ec-cb861b4b3224)

### ✅ Step 2: Create a New Azure Container Registry (ACR)

- Clicked on **Create**.
- Filled in the following configuration:
  - **Resource Group:** `csi_devops_acr`
  - **Registry Name:** `csitask3acr` *(unique name)*
  - **Location:** Central India
  - **SKU:** Basic (sufficient for this use case)

![Screenshot 2025-06-14 230301](https://github.com/user-attachments/assets/4e0befa0-1e33-4349-a440-043421cc5c18)


- After validating the details, I clicked **Review + Create**, then **Create**.

![Screenshot 2025-06-14 230323](https://github.com/user-attachments/assets/823bef8a-b14f-46a0-93ca-f688bea8e2d2)

### ✅ Step 3: Log in to Azure & ACR

- Logged in to Azure using the CLI:

```bash
az login
```
![Screenshot 2025-06-14 230343](https://github.com/user-attachments/assets/c622aea3-903b-4660-9dff-aa7c943c0f16)

- Then, authenticated with my Container Registry:

```bash
az acr login --name csitask3acr
```

###  Step 4: Tag & Push Docker Image to ACR

- I had already built a custom Node.js image locally. I tagged it using the full ACR path and pushed it to ACR

```bash
podman tag csitask3image csitask3acr.azurecr.io/csitask3image:v1
podman push csitask3acr.azurecr.io/csitask3image:v1
```
![Screenshot 2025-06-14 230401](https://github.com/user-attachments/assets/793f97ff-fd2f-47ae-ac7c-55f0950ab630)


### Step 5: Verify Image in ACR
- After the push was successful, I verified the image inside the Azure Portal under the Repositories section of my registry.

![Screenshot 2025-06-14 230422](https://github.com/user-attachments/assets/a1fd2fcd-bb57-44c8-9ea8-a0bd49b76fcf)


## Deploy Container from ACR Using Azure Container Instances

### Step 6: Create a Container Instance
- Navigated to Container Instances in the portal and initiated a new deployment:

  - **Container Name:** csitask3-container
  - **Image Source:** Azure Container Registry
  - **Image:** csitask3acr.azurecr.io/csitask3image:v1
  - **Port:** 3000 (used by my Node.js app)

![Screenshot 2025-06-14 230446](https://github.com/user-attachments/assets/56abc516-ff1d-44b3-9e3f-1b1b904b9755)


### Step 7: Review & Deploy
- Reviewed the configuration and clicked Create.

![Screenshot 2025-06-14 230508](https://github.com/user-attachments/assets/0bdd4b28-027e-4715-b623-dfdcdc848d08)


### Step 8: Container Deployed Successfully
- Once the deployment was complete, I navigated to the instance to confirm the status.

![Screenshot 2025-06-14 230653](https://github.com/user-attachments/assets/6ca8e121-f3f1-4bd6-877f-5eb0b8f49bd9)

### Step 9: Verify Web Application
- Finally, I copied the container’s public IP address, opened it in a browser, and confirmed that my Node.js app (serving a static HTML page) was running successfully.

![Screenshot 2025-06-14 231024](https://github.com/user-attachments/assets/cac40f77-29e5-4ffc-9766-0f4e9778510a)


---

## Conclusion

This task helped me gain practical experience with managing container images using Azure Container Registry, and deploying them easily through Azure Container Instances.

---
