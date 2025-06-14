# ☁️ Week 2 – Azure Compute: Task 2

## 📌 Task: Create App Service Plan & Deploy Static Web App

## 🎯 Objective

To understand and implement the deployment of a web application using Azure App Service Plan. In this task, I created a Linux-based **App Service Plan** and deployed a **Static Web App** using GitHub integration.

---

## 🧩 Step-by-Step Implementation

### Step 1: Navigate to App Service Plans

- From the Azure Portal home page, I searched and opened **App Service Plans**.

![Screenshot 2025-06-14 200822](https://github.com/user-attachments/assets/28eed4e4-9f96-4771-825e-cf780dfa0095)

### Step 2: Create a New App Service Plan

- Clicked **Create** to initiate the creation of a new plan.
- **Resource Group:** `CSI-DevOps-AppServices` (created a new group)
- **Name:** `task2-service-plan`
- **OS:** Linux
- **Region:** Canada Central / Central India
- **Pricing Tier:** Basic Plan B1: To integrate Continous deployment

![Screenshot 2025-06-14 200835](https://github.com/user-attachments/assets/e06ee256-cc2e-40bd-902d-cc2cd16c4ee3)


### Step 3: Review and Create

- After filling out the details, I clicked on **Review + Create** and verified all values.

![Screenshot 2025-06-14 200846](https://github.com/user-attachments/assets/db8f9caa-b983-4c9f-9830-9ffafc1146a2)

- Clicked **Create** and waited for successful deployment.

![Screenshot 2025-06-14 200901](https://github.com/user-attachments/assets/399d3324-a52f-47df-814c-2794fb9dc3dc)


### Step 4: Navigate to App Services

- After creating the plan, I went to **App Services** in the Azure portal and selected **Create** to provision a new app.

![Screenshot 2025-06-14 200911](https://github.com/user-attachments/assets/01365b3e-66dc-441a-9ecc-16aebb680148)


### Step 5: Create a Web App Using the Existing App Service Plan

- Navigated to **App Services** and clicked on **Create**.
- Chosen the following configuration:
  - **Subscription:** Selected existing subscription.
  - **Resource Group:** `CSI_DevOps_AppService` (same as earlier).
  - **Name:** `task2-simple-webapp`
  - **Publish:** Code
  - **Runtime stack:** Node.js 20 LTS
  - **Operating System:** Linux
  - **Region:** Central India / Canada Central
  - **App Service Plan:** Selected existing `task2-service-plan2`

![Screenshot 2025-06-14 200924](https://github.com/user-attachments/assets/c77b5b77-19ed-43ca-b29e-351905e82144)

### Step 6: Deployment via GitHub Actions

- In the **Deployment** tab during Web App creation, selected:
  - **GitHub** as the deployment source.
  - Authorized and selected the `csi-test-repo` repository and branch (`main`).

![Screenshot 2025-06-14 200935](https://github.com/user-attachments/assets/03d86e84-25d4-48f6-9d3e-eabc002cce9c)

### Step 7: Review + Create

- Verified all the configuration details.
- Clicked on **Create** and waited for the deployment to complete.

![Screenshot 2025-06-14 200945](https://github.com/user-attachments/assets/c2d3e871-3c57-432c-8b83-ed416a4fd3cb)


### ✅ Step 8: App Service Created Successfully

- After clicking **Create**, the Web App `task2-simple-webapp` was successfully provisioned using the selected App Service Plan.
- Navigated to **App Services > task2-simple-webapp** to confirm that the app was created.
- Azure automatically set up GitHub Actions for deployment as part of the selected deployment method.

![Screenshot 2025-06-14 200958](https://github.com/user-attachments/assets/8231fae8-4987-4e72-afcc-a71aff8b2ca3)


### ✅ Step 9: Azure App Service Started Deploying from GitHub

- Azure initiated deployment using the configured GitHub repository.
- Viewed deployment progress under **Deployment Center**.
- Verified that GitHub Actions workflow ran successfully, deploying the app code.

![WhatsApp Image 2025-06-14 at 23 42 39_f731aa80](https://github.com/user-attachments/assets/c7919485-a9b5-4855-853c-188d15b17381)


### ✅ Step 10: Final Web Page Rendered on Azure

- Accessed the Web App’s live URL (`https://task2-simple-webapp-akg8dchvc7d3c6ax.canadacentral-01.azurewebsites.net/`) by copying default domain.
- Confirmed that the static HTML page was served successfully from Azure App Service.

![Screenshot 2025-06-14 201048](https://github.com/user-attachments/assets/e725687e-b923-4dfd-a953-81320f026255)

---

## Conclusion

This task helped me learn how to create an App Service Plan and deploy a simple HTML web app using Azure. I was able to set up everything from the portal, connect my GitHub repo, and see my webpage live on Azure.

---
