# 🌐 Week 2 – Azure Storage: Task 9

## 📆 Task: Explore Azure Storage Account Capabilities

In this task, I explored Azure Storage Account features such as blobs, file shares, access controls, tiers, lifecycle policies, replication, and Azure File Sync. Here's what I did step-by-step, with real hands-on execution.

---

## Step 1: Created a Storage Account

I Navigated to the Azure portal and started the process of creating a new Storage Account. I named it `csidevopsstorage` and selected `East-US` as the region to keep it local.

![Screenshot 2025-06-15 141707](https://github.com/user-attachments/assets/86a268f6-37a3-4888-8ead-7ef38f70ffb7)


- I chosen the Standard performance tier for cost efficiency.

- For redundancy, I picked Read-access geo-redundant storage (RA-GRS) to make sure my data is highly available, even in case of regional outages.

- I left the default Access Tier as Hot since I wanted frequent access during testing.

Once created, the account was ready to host different types of data.

![Screenshot 2025-06-15 141718](https://github.com/user-attachments/assets/642987d7-188d-494d-a42c-d43763fe2c59)

## Step 2: Created a Blob Container and Uploading Data

Next, I went to the storage account and navigated to Containers. I created a container named `csi-devops-blob` and set its access level to Private for now to keep data secure.

![Screenshot 2025-06-15 141731](https://github.com/user-attachments/assets/fd9cd45c-2dc2-44b0-a872-46463fbac391)

Then, I uploaded a test file called `week2.txt, vikas-resume.docx` into the container.

- The file uploaded successfully, and I could see it listed under blobs.

![Screenshot 2025-06-15 141741](https://github.com/user-attachments/assets/27ce2797-f749-4cb6-83a1-196530af012c)


- I also checked blob properties and used the URL to confirm that access is restricted due to private mode.

![Screenshot 2025-06-15 141750](https://github.com/user-attachments/assets/6f930c22-841b-4d30-a43a-4c259d4b3c90)

![Screenshot 2025-06-15 141759](https://github.com/user-attachments/assets/869c8f30-f70e-4c11-85ed-3831cf0a22e9)


## Step 3: Explored Authentication Techniques

To understand how access control works in Azure Storage, I tested multiple authentication methods.

### A. Using Access Keys & Viewed by Storage explorer

- I picked one of the two Access Keys available under the Access Keys tab.

![Screenshot 2025-06-15 141813](https://github.com/user-attachments/assets/ac8cbd6b-abb8-45df-99da-2c4502ffb4f7)


- Then, I connected my storage account to Azure Storage Explorer using the key.

![Screenshot 2025-06-15 141827](https://github.com/user-attachments/assets/2bb25272-92cc-46f0-8a4c-9763c9fb552d)


- Through this, I was able to upload and download blobs without issues.

![Screenshot 2025-06-15 141855](https://github.com/user-attachments/assets/57f79a5f-bcdb-45ea-b7cd-26443f6fdc8a)

### B. Using Shared Access Signature (SAS, Storage Account Level)

- I generated a SAS token with Read and Write permissions, a limited IP range, and a short expiration time.

![Screenshot 2025-06-15 141908](https://github.com/user-attachments/assets/343f0e31-a86d-4583-86dd-e513340adedb)

- I tested the generated URL in the storage explorer, and it worked—only within the defined scope.

![Screenshot 2025-06-15 141922](https://github.com/user-attachments/assets/215a9c48-052c-4c51-a014-914c47785bd6)

![Screenshot 2025-06-15 141932](https://github.com/user-attachments/assets/307d8573-5565-430e-a10d-763f7715a0b2)

### C. Stored Access Policy + SAS (Container + Blob Level)

- I created a Stored Access Policy at the container level.

![Screenshot 2025-06-15 141942](https://github.com/user-attachments/assets/930f4cd3-f628-41d1-951e-abeac58b0a53)

- Then, I generated a SAS token tied to that policy to access a specific blob if require we can generate SAS token for container level as well.

![Screenshot 2025-06-15 141954](https://github.com/user-attachments/assets/39174677-1c36-4814-8e72-54d5a9f5c90f)

- This gave me more centralized control over the access scope and expiration.
- Now i can able to view the blob in browser

![Screenshot 2025-06-15 142006](https://github.com/user-attachments/assets/0d02182f-f2a3-4bc7-bce3-8df751dc8e4e)


## Step 4: Access Tiers

- Switched between **Hot**, **Cool**, **Cold** and **Archive** tiers for uploaded blob
- Observed:

  - **Hot**: Fast access, higher cost, Ideal for frequently accessed data.
  - **Cool**: For infrequently accessed data but still needed online, cheaper.

![Screenshot 2025-06-15 142015](https://github.com/user-attachments/assets/2d375fed-238d-4d55-ae10-4df773365b9d)

![Screenshot 2025-06-15 153945](https://github.com/user-attachments/assets/68b46221-1002-4d5d-a7e8-e15d38d44b40)


  - **Cold**: For data that is rarely accessed, cheaper than Hot and Cool, but slower

 ![Screenshot 2025-06-15 142044](https://github.com/user-attachments/assets/bba2e613-1298-4cc8-938b-394a77bd15b6)


  - **Archive**: Meant for long-term backup and is offline by default.

 ![Screenshot 2025-06-15 142054](https://github.com/user-attachments/assets/b596a4bc-7e3a-422b-9f8a-88d12993d9c2)


  - After moving to Archive, I tested rehydration to bring the blob back online, which took some time.

> Rehydration -> The process of transitioning an archived blob back to an online tier (Hot or Cool) so it becomes accessible again.

## Step 5: Lifecycle Management Policies

To automate data transitions and cleanup, I created a lifecycle management policy, So I configured a rule targeting **base blobs** (i.e., current active blobs), with the following actions:

- **Move to Cold** tier after **7 days**
- **Move to Archive** tier after **20 days**
- **Delete** the blob after **90 days**

This ensures my main blobs follow a cost-efficient storage path without managing snapshots or versions.

![Screenshot 2025-06-15 142104](https://github.com/user-attachments/assets/8cec0c5a-b618-4760-9887-fca834dcb1ba)

## Step 6: Blob Replication

I explored Object Replication to keep data in sync across regions.

- Created a two storage accounts named as `objectreplica1` `objectreplica2`.

![Screenshot 2025-06-15 142115](https://github.com/user-attachments/assets/0d1c1ebe-7fdf-4be6-bbd0-b2e6ff5372ff)


- After that created two containers named as `replica1sourcecontaienr` `replica2destinationcontainer`.

![Screenshot 2025-06-15 142125](https://github.com/user-attachments/assets/fafbe1c6-d7d0-4282-b574-14c4afb12068)


![Screenshot 2025-06-15 142132](https://github.com/user-attachments/assets/602f000e-2a7b-4e22-b932-60d2ed1d2499)


- Enabled replication by configuring the source and destination pairing.

![Screenshot 2025-06-15 142140](https://github.com/user-attachments/assets/4a4a8734-c356-49b3-9ee2-5bb91976a96f)


- Uploaded a file in the `objectreplica1` of `sourcereplicacontainer`

![Screenshot 2025-06-15 142154](https://github.com/user-attachments/assets/cf0860ca-28af-4f5e-9d07-ff82042e1282)


- Successfully replicated same file automatically into `objectreplica2` of `replica2destinationcontainer`

![Screenshot 2025-06-15 142203](https://github.com/user-attachments/assets/b1f791cb-304c-4cb2-9880-acf9deaa1c82)


![Screenshot 2025-06-15 142218](https://github.com/user-attachments/assets/af1acad7-ecb5-43a0-b52d-3d0c8d14c6b8)


> If we want two-way replication, we have to set up another rule in the opposite direction as well.

## Step 7: Azure File Share & File Sync

### Created File Share

- Created a file share named `devopsfileshare` in the storage account.

![Screenshot 2025-06-15 142234](https://github.com/user-attachments/assets/34d2b2bc-1b70-43ec-b18e-97bb6926d81a)


- Uploaded `mylinkedin` Image directly through the **Azure Portal**.

![Screenshot 2025-06-15 142307](https://github.com/user-attachments/assets/ad3ec338-9f99-41d5-9464-9208091c34bb)


### Connected via Azure File Sync

#### Windows Server Setup

- Deployed a **Windows Server VM**.

![Screenshot 2025-06-15 142318](https://github.com/user-attachments/assets/f422a2aa-e3be-42d0-9f22-5e2ca50e40d3)


- Installed the **Azure File Sync**.

![Screenshot 2025-06-15 142328](https://github.com/user-attachments/assets/406d501c-cff8-46fd-bd68-748a584fbf88)

#### Sync Test

- Linked a local folder on the VM to the file share.
- Successfully tested **bi-directional sync**:
  - ✅ Files created in the file share appeared on the server.

![Screenshot 2025-06-15 142355](https://github.com/user-attachments/assets/815da8e5-46c3-47fa-8cf0-ddafd38597e4)

  - ✅ Files added locally were synced back to Azure.

![Screenshot 2025-06-15 142416](https://github.com/user-attachments/assets/34894fd6-2fe4-45e3-bc9d-6f183c4dcf79)


## Conclusion

This task gave me a complete overview of how Azure handles various types of storage needs. I tested authentication techniques, access tiers, policies, replication, and file synchronization. These are critical for managing enterprise data efficiently and securely.
