🚀 Deploy Docker App using Azure Container Instances (ACI) – Full Guide
📌 What You’ll Learn:
Prepare and use Docker images (from Docker Hub or ACR)

Create Azure Container Instances via Portal or CLI

Deploy & expose containerized apps

Monitor logs

Clean up resources

🧱 Step 1: Prepare Your Docker Image
✅ Option A: Use a public image from Docker Hub
Example:

bash
Copy
Edit
mcr.microsoft.com/azuredocs/aci-helloworld
This image serves a basic webpage on port 80.

✅ Option B: Push your own image to Azure Container Registry (ACR)
🔹 1. Create a Resource Group:
bash
Copy
Edit
az group create --name MyDockerRG --location eastus
🔹 2. Create ACR (Azure Container Registry):
bash
Copy
Edit
az acr create --resource-group MyDockerRG \
  --name myacr12345 \
  --sku Basic
🔹 3. Login and Push Image:
bash
Copy
Edit
az acr login --name myacr12345
docker tag myapp myacr12345.azurecr.io/myapp
docker push myacr12345.azurecr.io/myapp
🧱 Step 2: Create Azure Container Instance (ACI)
✅ Option A: Using Azure Portal
Go to Azure Portal

Navigate to Container Instances → Create

Fill in the form:

Resource Group: MyDockerRG

Container Name: mycontainerapp

Image Source:

Public: mcr.microsoft.com/azuredocs/aci-helloworld

Private: Select your ACR

DNS Name Label: mydockerdemo

Port: 80

Click Review + Create

✅ Option B: Using Azure CLI
If using Public Image:
bash
Copy
Edit
az container create \
  --resource-group MyDockerRG \
  --name mycontainerapp \
  --image mcr.microsoft.com/azuredocs/aci-helloworld \
  --dns-name-label mydockerdemo \
  --ports 80 \
  --location eastus
If using ACR Private Image:
bash
Copy
Edit
az container create \
  --resource-group MyDockerRG \
  --name mycontainerapp \
  --image myacr12345.azurecr.io/myapp \
  --registry-login-server myacr12345.azurecr.io \
  --registry-username <acr-username> \
  --registry-password <acr-password> \
  --dns-name-label mydockerdemo \
  --ports 80
💡 Tip: You can fetch credentials with:

bash
Copy
Edit
az acr credential show --name myacr12345
🌐 Step 3: Access Your Application
Once deployment is successful, open in browser:

arduino
Copy
Edit
http://mydockerdemo.eastus.azurecontainer.io
You should see your app or the default web page from the image.

📊 Step 4: Monitor Container Logs
Use this to debug and verify runtime output:

bash
Copy
Edit
az container logs \
  --resource-group MyDockerRG \
  --name mycontainerapp
To check container status:

bash
Copy
Edit
az container show \
  --resource-group MyDockerRG \
  --name mycontainerapp \
  --query instanceView.state
🧹 Step 5: Clean Up Resources
Delete only the container:
bash
Copy
Edit
az container delete --name mycontainerapp \
  --resource-group MyDockerRG --yes
Delete the entire resource group:
bash
Copy
Edit
az group delete --name MyDockerRG --yes --no-wait
📚 Summary Table
Task	                     Tool	               Command/Action
Create RG	               Azure CLI	            az group create
Create ACR	Azure CLI	az acr create
Push Docker Image	Docker + ACR	docker push
Create Container Instance	CLI/Portal	az container create
Access App	Web Browser	Use DNS URL
Monitor Logs	Azure CLI	az container logs
Clean Up	Azure CLI	az group delete

🎓 Best Practices




Use Azure Resource Naming Conventions

Always enable authentication for ACR

Monitor usage and pricing in Azure Portal

Use Azure Monitor for advanced logging

Store credentials using Azure Key Vault
