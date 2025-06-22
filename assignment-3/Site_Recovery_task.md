# 🛡️ Week 3 Task 8 – Azure Site Recovery (ASR)

## 🎯 Objective

In this task, I set up **Azure Site Recovery (ASR)** to enable disaster recovery for my existing VM (`monitoring-vm`). This ensures that, in case of a regional outage, the VM can be failed over to a secondary region with minimal downtime.

**Note**: In this week, there were a few repeated questions and no specific task assigned on Site Recovery. So, I took the initiative to explore Azure Site Recovery in depth to get hands on experience and better understanding about disaster recovery in Azure

![Screenshot 2025-06-22 113256](https://github.com/user-attachments/assets/8cf9a859-5bf6-4e90-ba8e-51cb7cf3cf17)

---

## ✅ Step-by-Step Implementation

### Step 0: Reuse VM for ASR Configuration

I reused the same VM (`monitoring-vm`) that I had created and used for monitoring and backup.

- **VM Name:** `monitoring-vm`
- **OS:** Ubuntu Server
- **Region:** Central India
- **Size:** Standard B2s (2 vCPU, 1 GiB RAM)
- **Resource Group:** `monitoring-rg`

This kept everything consistent and avoided extra provisioning steps.

### Step 1: Prepare Permissions and Infrastructure

Before proceeding, I made sure:

- My Azure account had **Owner/Contributor** access in both source and target subscriptions.
- I assigned the **Site Recovery Contributor** role to my account on the vault.
- The VM had the **Azure VM Agent** installed and outbound network access.

### Step 2: Create Recovery Services Vault for ASR

- Navigated to **Azure Portal → Recovery Services Vaults → + Create**
- Entered the following:

  - **Name:** `csi-devops-asr-vault`
  - **Resource Group:** `monitoring-rg`
  - **Region:** **West India** (a different region from the source)

- Clicked **Review + Create**, then **Create**
- Vault creation completed successfully

![Screenshot 2025-06-22 113322](https://github.com/user-attachments/assets/a0526d65-8d13-46ad-998f-c06cee46f023)

### Step 3: Enable Site Recovery

- Opened `csi-devops-asr-vault` → clicked **Site Recovery** → **Enable Replication**

![Screenshot 2025-06-22 113335](https://github.com/user-attachments/assets/d3e3d4e4-4fe1-4b42-a9cb-97b5152b7613)


- Configured source settings as follows:

  - **Source Region:** Central India
  - **Subscription & Resource Group:** my existing ones
  - **Deployment Model:** Azure Resource Manager
  - **Replica Model:** No Availability Zone

Clicked **Next** to proceed.

![Screenshot 2025-06-22 113351](https://github.com/user-attachments/assets/f08166ff-cbc6-40a9-ab90-c5e8f25e4b48)


### Step 4: Select the Virtual Machine

- On the next screen, I selected the VM `monitoring-vm` to replicate
- Clicked **Next** to confirm the selection

![Screenshot 2025-06-22 113405](https://github.com/user-attachments/assets/ba1a880d-8b6c-4808-874b-4ca19de0073f)


### Step 5: Configure Target Settings

- Selected **Target Subscription** and **EAST ASIA** region
- Chose a virtual network in East Asia for failover
- Accepted the automatic provisioning of the necessary storage account and network interfaces
- Clicked **Next**

![Screenshot 2025-06-22 113416](https://github.com/user-attachments/assets/089b89e4-ba1d-42a3-9a34-1e9a66887da4)

### Step 6: Adjust Replication Policy

- Viewed the default replication policy, which includes:

  - Application-consistent snapshot frequency
  - RPO retention (default 24 hours)

- I kept the default settings, which are suitable for my test environment
- Azure Site Recovery also supports using Replication Groups to group multiple VMs for coordinated replication and failover. However, for this task, I focused on replicating a single VM and did not configure a replication group.
- Clicked **Next** to move on

![Screenshot 2025-06-22 113426](https://github.com/user-attachments/assets/583efeb8-e6e6-4408-8b9c-3365bcd70120)


### Step 7: Enable Replication

- Reviewed all configurations

![Screenshot 2025-06-22 113436](https://github.com/user-attachments/assets/5e653d75-94d1-4029-ba3b-71ecd145815a)

- Clicked **Enable Replication**
- Azure automatically installed the ASR Mobility agent on the VM, set up resources, and triggered the initial replication

![Screenshot 2025-06-22 113446](https://github.com/user-attachments/assets/92f3382c-f6eb-4a3d-b0f1-caf233284d05)


## Step 8: Confirm VM Is Replicating

- Once replication started, I checked **`Vault → Replicated Items`**
- The VM `monitoring-vm` appeared with a **Healthy** status.
- This confirms that the VM is actively replicating to the target region

![Screenshot 2025-06-22 113457](https://github.com/user-attachments/assets/69885b75-2c5b-42df-8cd8-96691bece995)


- Infrastructure view

![Screenshot 2025-06-22 113508](https://github.com/user-attachments/assets/16774a00-af6c-4651-b326-5164ae50d2d9)


---

## Azure Site Recovery – Test Failover Validation

After setting up Azure Site Recovery for my VM `monitoring-vm`, I wanted to verify if everything works as expected in case of a disaster. So, I performed a **Test Failover** to validate the configuration without affecting my running production VM.

### Step 9: Run a Test Failover for `monitoring-vm`

Once replication was marked as healthy under the **Replicated Items** section of my Recovery Services Vault, I proceeded with a test failover.

### Why Run a Test Failover?

A test failover simulates a real disaster recovery scenario by spinning up a **temporary copy** of the VM in the **target region** (East Asia in my case), using the latest replicated data. It helps ensure:

- The VM boots up successfully
- Operating system and application layers function normally
- Networking and IP configurations are compatible

Most importantly, this test is isolated — it **does not impact the source VM**.

#### Start Test Failover

- Clicked on **Test Failover** at the top

![Screenshot 2025-06-22 113523](https://github.com/user-attachments/assets/38bf713b-2e28-450e-ad1e-e86035286428)


- Chose the following options:
  - **Recovery Point:** Latest processed
  - **Target Region:** Already set to East Asia
  - **Target VNet:** Selected the test failover VNet provisioned during replication

Then I clicked to initiate the failover.

![Screenshot 2025-06-22 113536](https://github.com/user-attachments/assets/de78bc1a-ab29-4a9f-b962-7def70ea9da3)


> Azure started the process of creating a temporary test VM in the East Asia region.

![Screenshot 2025-06-22 113548](https://github.com/user-attachments/assets/59a22ebb-217e-4f7b-b8ec-551d9d5d8106)

![Screenshot 2025-06-22 113601](https://github.com/user-attachments/assets/c20da5a7-250e-4d52-a2a9-7652b7281a26)


### Step 10: Clean Up Test Failover Resources

Since the test VM created during the failover is only for validation purposes, it's important to **clean up** these temporary resources to avoid **unnecessary costs** and **resource clutter**.

- Clicked on **"Cleanup Test Failover"** from the top menu

![Screenshot 2025-06-22 113613](https://github.com/user-attachments/assets/b9a889a8-b644-4b56-bcee-cf79b45aa95f)


In the prompt:

- I entered the comment:
  > `"Test passed successfully"`
- Confirmed the cleanup

![Screenshot 2025-06-22 113632](https://github.com/user-attachments/assets/0e389a65-7288-40f2-9936-f2211f6dba6b)


Azure automatically deleted the temporary test VM and associated resources from the **target region** (East Asia), ensuring everything is clean and no leftover test infrastructure remains.

![Screenshot 2025-06-22 113642](https://github.com/user-attachments/assets/b9c2f2a1-9ed9-4662-b066-4475813f0ede)


![Screenshot 2025-06-22 113656](https://github.com/user-attachments/assets/06a08200-1dec-47d2-85fc-4eabcb8597a4)


> ✅ This final step completed the test failover lifecycle and confirmed the replication setup works as expected.

---

## Conclusion

With Azure Site Recovery configured, my VM is now protected against regional outages. Replication is active, and I can easily initiate a test or actual failover when required, ensuring business continuity and minimal disruption.

---
