# Week 8 – Advanced Pipelines & Self-Hosted Agent Setup (Azure DevOps)

## Objective

The main goal of this task was to set up a **Linux-based self-hosted agent** in Azure DevOps. This gave me full control over the build environment, allowed custom tooling, and enabled faster debugging for CI/CD pipelines. I used an Ubuntu 22.04 VM, registered it with a custom agent pool, and successfully verified it by running a test pipeline.

---

### Step 1: Provisioning the Ubuntu Virtual Machine

I started by provisioning a virtual machine in my cloud environment using Ubuntu 22.04 as the base image. Once the provisioning was complete and the VM was up and running,

<img width="1918" height="1028" alt="vm-created" src="https://github.com/user-attachments/assets/ccbb9eec-9791-4232-aa14-99f82ec96b50" />


I accessed it via SSH from my local system.

```bash
ssh azureuser@<your-vm-public-ip>
```

<img width="1918" height="888" alt="logged-to-vm" src="https://github.com/user-attachments/assets/582da43c-8439-49aa-805c-c3eca6895a49" />


---

### Step 2: Creating a Self-Hosted Agent Pool in Azure DevOps

After logging into the VM, I moved to my Azure DevOps Organization to configure a new agent pool for self-hosted agents:

1. Navigated to my **Azure DevOps Project**
2. Clicked on **Project Settings** → **Agent Pools** (under Pipelines)

<img width="1918" height="1027" alt="csi-agent-pool" src="https://github.com/user-attachments/assets/5a3c958f-23b2-4f5e-9fc3-7d78d780eab7" />


3. Selected **"Add Pool"**, gave it the name: `csi-hosted-agent-pool`
4. Chose **"Self-hosted"** as the agent type

<img width="1912" height="1025" alt="self-hosted-pool" src="https://github.com/user-attachments/assets/c9a5c409-9d07-487e-a828-f54c1e9cf156" />


5. Clicked **Create** to finalize the pool

<img width="1918" height="983" alt="agent-pool-created" src="https://github.com/user-attachments/assets/00159d39-ffe7-4571-8b1e-879409157dec" />


This pool would act as the container to register and manage any custom-hosted agents.

---

### Step 3: Installing the Linux Agent on the VM

Once the pool was created, I clicked on new agent to create, Azure DevOps provided setup instructions based on the OS.

I selected **Linux** and followed the steps as below:

<img width="1918" height="1027" alt="new-agent" src="https://github.com/user-attachments/assets/8440b246-350a-4960-ac34-74a895ef43b6" />


1. **Downloaded the agent package** using `wget`:

   ```bash
   wget https://vstsagentpackage.azureedge.net/agent/3.236.0/vsts-agent-linux-x64-3.236.0.tar.gz
   ```

2. **Created a dedicated directory** for the agent:

```bash
mkdir myagent && cd myagent
```

3. **Extracted the downloaded archive**:

```bash
tar zxvf ../vsts-agent-linux-x64-3.236.0.tar.gz
```

<img width="1918" height="1062" alt="downloaded-agent" src="https://github.com/user-attachments/assets/8889b2b0-b74e-4ed6-8850-446cc73d9d46" />


---

### Step 4: Configuring the Agent (./config.sh)

To initialize the agent, I ran the following command:

```bash
./config.sh
```

This interactive setup asked me for a few things:

* **Organization URL**: I provided my Azure DevOps organization URL → `https://dev.azure.com/vikasprince`
* **Authentication Type**: I chose `PAT (Personal Access Token)`

Then, I went back to the Azure DevOps portal:

1. Clicked on the **user icon (top-right)** → **Personal Access Tokens**
2. Created a new token named `csi-agent-pat` (for demo/testing)
3. Granted **full access** for initial setup (Note: this is **not recommended** for production; use scoped access)


<img width="1918" height="950" alt="pat-created" src="https://github.com/user-attachments/assets/8efbb507-40c5-4417-a6b9-739bcdc87778" />



Back on the VM, I entered the PAT when prompted and continued with the setup:

* Provided **Agent Pool**: `csi-hosted-agent-pool`
* Named the Agent: `csi-agent`

Once the configuration completed successfully, the agent was now registered with Azure DevOps.

<img width="1918" height="1047" alt="hosted-agent-created" src="https://github.com/user-attachments/assets/6426b0f8-0636-45e4-a4a0-d5d95ef0e2df" />


---

### Step 5: Starting the Agent (./run.sh)

To run the agent so it can pick up jobs from the pipeline, I executed:

```bash
./run.sh
```

![a<img width="1918" height="1070" alt="run-agent" src="https://github.com/user-attachments/assets/fcaab08b-6ff5-410a-9c9e-75b3d22442c3" />


This started the agent and it began polling Azure DevOps for available jobs.

---

### Step 6: Verification from Azure DevOps Portal

To verify if everything worked correctly:

1. I navigated back to the Azure DevOps portal
2. Went to **Agent Pools** → `csi-hosted-agent-pool`
3. There I saw the `csi-agent` showing as **Online** 

<img width="1918" height="686" alt="csi-agent-online" src="https://github.com/user-attachments/assets/0aa4fe31-de6b-4c6c-9b8e-ad42fa363d71" />


This confirmed that my self-hosted agent was live and ready to handle builds and releases.

---

### Step 7: Running a Test Pipeline

With my agent successfully connected and verified, I created a simple test pipeline. I targeted the custom agent pool to confirm the build agent could execute jobs seamlessly.

```bash
pool:
  name: csi-hosted-agent-pool

steps:
  - script: echo "Running from self-hosted agent"
    displayName: "Verify Self-Hosted Agent"
```

<img width="1918" height="902" alt="test-pipeline" src="https://github.com/user-attachments/assets/2d105a36-e0e5-4433-8620-f5e8b70db179" />


This job executed successfully on the self-hosted agent, marking the completion of Task.

<img width="1918" height="907" alt="agent-job-success" src="https://github.com/user-attachments/assets/9bde972b-f40d-4f54-8bf1-0f3645a9e26f" />


Finally to verify that weather those jobs are correctly executed under created self hosted agent pool, for that i went to agent pools under `csi-agent-pool` all executed jobs are listed. Hence my self hosted linux agent working as expected

<img width="1918" height="973" alt="job-list" src="https://github.com/user-attachments/assets/fde51db5-f511-4e53-a45a-199c0518edcd" />


With the self-hosted agent in place, I used it for all upcoming tasks in this week — including pipeline variables, variable groups, scoped values per environment, and gated release approvals.

---

### Conclusion

Setting up the self-hosted agent was smooth and straightforward. After configuring and running the agent, I confirmed that it was online and able to execute pipelines from Azure DevOps. This setup will now serve as the backbone for all advanced tasks, making my builds more efficient and flexible.

---
