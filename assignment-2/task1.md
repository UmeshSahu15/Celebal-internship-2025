
# ☁️ Week 2 – Azure Compute: Task 1

## 🚀 Task: Deploy Linux and Windows Virtual Machines & Access via SSH/RDP

## 📌 Objective

In this task, I deployed both a **Red Hat Linux** and a **Windows Server** virtual machine on Microsoft Azure. I accessed the Linux machine using **SSH keys** and the Windows machine using **Remote Desktop (RDP)**. I’ve included step-by-step screenshots and descriptions of everything I’ve done.

---

## Part 1: Deploying Linux VM and SSH Access

### Step 1: Open Azure Portal and Go to Virtual Machines

I logged in to the Azure portal and navigated to **Virtual Machines**.

![IMG-20250614-WA0012](https://github.com/user-attachments/assets/b1fc0710-e98a-44e8-93bb-f64c6fbc44e1)

### Step 2: Create Resource Group

Before creating any resource, We must need to create a resource group. So I created a resource group named `csi-devops` to logically group my resources.

![Screenshot 2025-06-14 195559](https://github.com/user-attachments/assets/8a90209e-1f68-4fd0-8290-89eb33ac7481)


### Step 3: Configure Linux VM Basics

I am going to create linux server 

- Chose OS: **Red Hat Enterprise Linux 9**
- VM Size: **Standard_B1ls** (1 core, 1 GiB RAM)
- Username: `csi_user`
- Authentication: **SSH Key**
- Key pair name: `csi-devops-linux_key.pem` (downloaded `.pem` file)

![Screenshot 2025-06-14 195615](https://github.com/user-attachments/assets/6d21dc59-9637-48ba-8000-34e1184762ef)


![Screenshot 2025-06-14 195635](https://github.com/user-attachments/assets/b9fdba89-0e49-4798-a30f-ebdfba93842a)

### Step 4: Networking Configuration

- Allowed **port 22** for (SSH)
- Attached **Public IP**, NSG, and default Virtual Network

![Screenshot 2025-06-14 195656](https://github.com/user-attachments/assets/2bd14ee8-621b-4a49-8d08-63a41bbf015b)

### Step 5: Review and Create

After final review, I clicked **"Create"**. The deployment completed successfully.

![Screenshot 2025-06-14 195733](https://github.com/user-attachments/assets/fdd3f937-8086-491e-8f7a-8cec7339a885)

![Screenshot 2025-06-14 195751](https://github.com/user-attachments/assets/5c1f11bb-3d25-4814-9a7e-0127af494d2c)

### Step 6: SSH into the Linux VM

- Navigated to Pem File location and Changed permission of `.pem` file:

```bash
chmod 400 csi_keypair.pem
```

- Finally Logged into Virtual Machine using public IP

```bash
ssh -i csi-devops-linux_key.pem csi_user@20.2.67.140
```

![Screenshot 2025-06-14 200600](https://github.com/user-attachments/assets/755ff8a5-89c0-4195-a8cf-e8a7ef173457)


---

## Part 2: Deploying Windows VM and RDP Access

### Step 1: Create a Windows Server VM

I went back to the Azure Portal and selected Create a virtual machine. This time, I set it up to run Windows Server 2019 Datacenter.

- OS Image: Windows Server 2022 Datacenter
- VM Size: Standard_B2s (2 vCPUs, 4 GiB RAM)
- Administrator Username: csi_user
- Authentication Type: Password
- Password: A strong password stored securely

![Screenshot 2025-06-14 195845](https://github.com/user-attachments/assets/a9bdae2b-95a3-441e-8383-efbf870ab240)

![Screenshot 2025-06-14 195859](https://github.com/user-attachments/assets/fb11d719-b5f3-4768-a073-0d0c19629732)


### Step 2: Configure Networking

- To enable Remote Desktop access, I allowed port 3389 (RDP) through the Network Security Group (NSG). The VM was also assigned a public IP and added to the default virtual network.

![Screenshot 2025-06-14 195910](https://github.com/user-attachments/assets/ba7c51c7-c177-481c-8b9f-c93237f3b68d)


### Step 3: Review and Deploy

- I reviewed all the configuration settings to make sure everything looked good, then clicked Create. Azure took a few minutes to deploy the VM.

![Screenshot 2025-06-14 195921](https://github.com/user-attachments/assets/1f13bca4-1efa-44d1-8306-49cd4a61b346)


### Step 4: Access Windows VM via RDP

Once the deployment was complete, I accessed the VM as follows:

- Navigated to the VM's Overview page in the portal.
- Clicked Connect > RDP and downloaded the .rdp file.
- Opened the .rdp file on my local machine.
- Entered the login credentials (azureadmin and the password) when prompted.

![Screenshot 2025-06-14 195941](https://github.com/user-attachments/assets/e79d6318-7c8f-4eb0-9d81-2090ac647b38)


- I successfully logged into the Windows Server desktop environment via Remote Desktop.

![Screenshot 2025-06-14 200012](https://github.com/user-attachments/assets/883bb0e5-605d-4bf9-a5f0-5d06faddb5ab)

---

## Conclusion

I deployed a Linux and a Windows virtual machine on Azure. The Linux VM was accessed using SSH keys, and the Windows VM was accessed using RDP. I configured the necessary settings, allowed required ports (22 for SSH and 3389 for RDP), and successfully connected to both VMs. This task provided practical experience with Azure VM deployment and remote access.
