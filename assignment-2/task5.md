# 🌐 Week 2 – Azure Networking: Task 5

## 📌 Task

### A. Create a VNet with 2 Subnets
- **Subnet-1:** For Linux VM & Windows VM  
- **Subnet-2:** For SQL DB

### B. Create 4 VNets
1. **Management VNet (Hub)**
2. **Production VNet (Spoke)**
3. **Testing VNet (Spoke)**
4. **Development VNet (Spoke)**

> Configure **Hub-and-Spoke Architecture** and validate the setup by launching VMs in each VNet and verifying connectivity by **pinging all Spoke VMs from Management VM**.

---

## 🎯 Objective

To understand Azure Virtual Networking concepts and implement **Hub-and-Spoke topology**, enabling centralized access and monitoring, which is a common enterprise network design in real-world DevOps infrastructure.

---

## Part A: Single VNet with Two Subnets

### Step 1: Create a VNet with Two Subnets

- Navigated to **Azure Portal → Virtual Networks → Create**
- Gave the VNet name as `task5-vnet`, region as `Central India`
- Created:
  - **Subnet-1**: `vm-subnet` (for Linux & Windows VMs)
  - **Subnet-2**: `db-subnet` (for future SQL resources)

![WhatsApp Image 2025-06-15 at 00 19 59_e987c729](https://github.com/user-attachments/assets/ceb816f8-f410-4b61-956d-d1d7d613cd3e)


![IMG-20250614-WA0031](https://github.com/user-attachments/assets/308000da-d415-42f1-a44e-f31a891c00e4)


### Step 2: Deployed VMs into Subnet-1

- Created:
  - **Linux VM:** Redhat Linux using username and password

 ![IMG-20250614-WA0039](https://github.com/user-attachments/assets/c2824b15-b99a-4329-bdb6-73ba987ffe1f)

 ![IMG-20250614-WA0045](https://github.com/user-attachments/assets/73769e3c-bb41-486c-9a4f-641d034cd779)

  - **Windows VM:** Windows Server 2019 using RDP

 ![IMG-20250614-WA0049](https://github.com/user-attachments/assets/dea62ee5-b372-4fb0-bb4a-75380151f410)

 ![IMG-20250614-WA0033](https://github.com/user-attachments/assets/d47be5aa-3309-4793-a18f-666701eb8489)

- Both VMs placed under `vm-subnet`

 ![IMG-20250614-WA0036](https://github.com/user-attachments/assets/669c2615-6a23-4617-8bca-10ba20742735)


### Step 3: Verified Access

- Logged into Linux VM using SSH

![IMG-20250614-WA0046](https://github.com/user-attachments/assets/6c8c3be8-fc2c-498a-a893-2461bac46fd7)


- Logged into Windows VM using RDP (enabled port 3389)

![WhatsApp Image 2025-06-14 at 23 41 31_1b7136af](https://github.com/user-attachments/assets/bf9ecb28-c628-4a18-886a-b1526599b0ce)


- Ensured both were reachable and configured properly

## Azure SQL Databas
I provisioned an Azure SQL Database and connected to it using Azure Data Studio. Here's everything I did, step by step:

### Step 1: Created Azure SQL Server
I first searched for SQL Server in the Azure portal and clicked on Create.
I provided the following details:
- Server Name: `csi-devops-sql-server`
- Region: Central India
- Authentication: SQL Authentication
- Admin Username: `sqladmin`
- Password: secure password

> This server will act as a container for the database I create next.

![IMG-20250614-WA0037](https://github.com/user-attachments/assets/60d455cc-6ac6-4d39-ac19-c1c91533d3c2)



### Step 2: Created SQL Database

Once the server was ready, I clicked on Create SQL Database and filled in the following:

- Resource Group: csi-devops
- Database Name: csi-devops-db
- Compute + Storage: General Purpose – Serverless (Gen5), 1 vCore, 32 GB
- Backup Redundancy: Geo-redundant (for high availability)

![IMG-20250614-WA0056](https://github.com/user-attachments/assets/f50366e7-a5bb-4e25-ae90-e64c155d4213)

### Step 3: Configured SQL Server Firewall
Before connecting, I navigated to the Networking tab of the SQL Server and:

- Enabled access from Azure services
- Added my client IP to the firewall rules

### Step 4: Review the changes and create
After reviewing the changes, I clicked on Review + create to create the database.
> I also selected to use the sample database provided by Azure for testing.

![IMG-20250614-WA0044](https://github.com/user-attachments/assets/664f2131-57b8-4f0b-852c-bae1362cd8a1)


- Successfully Deployed Sql Database

![IMG-20250614-WA0050](https://github.com/user-attachments/assets/cbfa82f8-8b5f-488c-bd08-b8a550da4527)

### Step 5: Connect to the database using Azure Data Studio
Once everything was ready, I opened Azure Data Studio and connected using:

- Server name: csi-devops-sql-server.database.windows.net
- Login: sqladmin
- Password: same as earlier

![IMG-20250614-WA0043](https://github.com/user-attachments/assets/be130716-d26d-4180-a5ce-be3fceca99fc)


After logging in, I added my Azure account and verified that the csi-devops-db was listed.

![IMG-20250614-WA0052](https://github.com/user-attachments/assets/a0578d62-47fb-4d5c-a2a8-0698047510de)


Connection was established successfully and able to saw the databases

![IMG-20250614-WA0054](https://github.com/user-attachments/assets/3d5a7b46-1bfe-4c1e-af81-b6ef98ba58da)


### Step 5: Queried the Sample Database

To validate the setup, I executed the following queries:

```bash
SELECT TOP 10 * FROM SalesLT.Customer;
SELECT * FROM SalesLT.SalesOrderHeader WHERE TotalDue > 1000;
```
- And it returned the expected results from the sample data, confirming a successful connection and working database.

![IMG-20250614-WA0051](https://github.com/user-attachments/assets/7f0eeaa5-a820-467c-b514-3c899039d8fa)


---


## Part B: Hub-and-Spoke Architecture

### Step 1: Created 4 VNets

I created the following VNets, all in the **Central India** region:

| VNet Name   | Address Space | Purpose           |
| ----------- | ------------- | ----------------- |
| `hub-vnet`  | 10.0.0.0/16   | Management Hub    |
| `prod-vnet` | 10.1.0.0/16   | Production Apps   |
| `test-vnet` | 10.2.0.0/16   | Testing Workloads |
| `dev-vnet`  | 10.3.0.0/16   | Development       |

![IMG-20250614-WA0053](https://github.com/user-attachments/assets/77dc8886-4413-4e7c-b183-7b30c3bccc76)


### Step 2: Configure Peering (Hub-and-Spoke)

- Peered:
  - `hub-vnet` → `prod-vnet`
  - `hub-vnet` → `test-vnet`
  - `hub-vnet` → `dev-vnet`
- Enabled **"Allow traffic to remote virtual network"**  in all peerings.

![IMG-20250614-WA0035](https://github.com/user-attachments/assets/72d6173f-295e-4576-933b-80fcb51dd018)



### Step 3: Deployed VMs in Each VNet

I deployed one **RedHat Linux VM** in each VNet:

| VNet        | VM Name   | Public IP |
| ----------- | --------- | --------- |
| `hub-vnet`  | `hub-vm`  | Yes       |
| `prod-vnet` | `prod-vm` | No        |
| `test-vnet` | `test-vm` | No        |
| `dev-vnet`  | `dev-vm`  | No        |

- Deployed Management Virtual Machine in `hub-vnet` with public IP

![IMG-20250614-WA0041](https://github.com/user-attachments/assets/ae1632a6-77df-4222-8930-ba491ac8a391)


- Deployed A Development Virtual Machine in `dev-vnet` without public IP

![IMG-20250614-WA0040](https://github.com/user-attachments/assets/a00f6e78-04c6-4dce-9341-d00648432325)


- Deployed A Production Virtual Machine in `prod-vnet` without public IP

![IMG-20250614-WA0038](https://github.com/user-attachments/assets/e4600164-e011-4986-bb6d-082334f8340a)

- Deployed A QA Virtual Machine in `QA-vnet` without public IP

![IMG-20250614-WA0034](https://github.com/user-attachments/assets/2f50ac29-8356-48e9-9ee1-0a77d67006dd)

---

### Step 4: Verified Connectivity from HUB VM

* I logged into the `hub-vm` from the `hub-vnet`
* Used `ping` to check connection to other VMs using their **private IPs**

```bash
ping dev-vm-private-ip
```

![IMG-20250614-WA0047](https://github.com/user-attachments/assets/632e1421-ba80-405a-8559-368437ed898a)



```bash
ping prod-vm-private-ip
```

![IMG-20250614-WA0055](https://github.com/user-attachments/assets/6484bfa1-9477-4f74-8287-61b6d2dbc378)



```bash
ping test-vm-private-ip
```

![IMG-20250614-WA0032](https://github.com/user-attachments/assets/fe643641-e923-4cd4-b62a-820a1c7b6ffc)


All VMs responded, which confirmed that the **Hub-and-Spoke topology** was working as expected.

---

## Conclusion

This task helped me understand Azure networking by creating VNets, subnets, and setting up a Hub-and-Spoke architecture. I deployed VMs in each VNet, configured peering, and verified that all spoke VMs were reachable from the hub VM. I also set up an Azure SQL Database, connected to it, and ran basic queries to confirm everything was working. Overall, it was a good hands-on experience with Azure network setup and connectivity.

---
