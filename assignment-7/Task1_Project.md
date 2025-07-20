
# Week 7 – Task 1: Azure DevOps

## Task: Project Creation & Group Policy Implementation

## Task Overview:
As part of Week 7 Task1, I explored the core project and permission management features of **Azure DevOps**. The focus was on **creating a new project**, **defining user groups**, and **applying proper group permissions** just like how it's done in real-time DevOps teams for maintaining access control, compliance, and collaboration.

---

## Objective:
- Create a new Azure DevOps project.
- Define multiple user groups (e.g., Developers, QA, Admins).
- Assign meaningful permissions to each group.
- Apply policies to restrict access or grant elevated roles where necessary.

---

## Step-by-Step Execution:

### Logged in to Azure DevOps Portal
- Accessed [https://dev.azure.com](https://dev.azure.com).
- Used my organizational credentials to log in.
- Selected the appropriate Azure DevOps **Organization** under which this new project will reside.


### Created a New Project
- Clicked on the **“New Project”** button on the Azure DevOps homepage.
- **Project Name:** `csi-azure-devops-week`
- **Visibility:** Kept it **Private** (since we want only internal team members to access it).
- **Advanced Options:**
  - Version Control: `Git`
  - Work Item Process: `Scrum`

![azure-devops-project](https://github.com/user-attachments/assets/e6d9d222-5ef7-4d4f-bc63-92a8f3208d17)


 Clicked **Create** and waited for the project workspace to initialize.

![project-created](https://github.com/user-attachments/assets/7d2e623c-8577-4160-bef1-f30b775016b2)


---

### Navigated to Project Settings
- Once the project was created, went to the **Project Settings** located at the bottom left corner of the screen.
- From here, began managing **Permissions**, **Groups**.

![project-settings](https://github.com/user-attachments/assets/39f4e79e-80b8-45ed-aa8c-9c446679e025)


---

### Created Custom User Groups
Azure DevOps provides default groups (Project Administrators, Contributors, Readers..), but I created specific custom groups for clearer access control:

#### Developers Group
- Created a group named `CSI-Developers-Team`
- Members: Added Myself as a Member

![develop-group](https://github.com/user-attachments/assets/254ca033-49e5-46c7-a178-e282f76b44dc)


#### General Permissions:
- **Delete team project**: Not allowed
- **Edit project-level information**: Not allowed
- **View project-level information**: Allowed
- **Update project visibility**: Not allowed

#### Boards:
- **Create tag definition**: Allowed
- **Bypass rules on work item updates**: Not allowed
- **Delete and restore work items**: Not allowed
- **Move work items out of this project**: Not allowed
- **Permanently delete work items**: Not allowed

#### Analytics:
- **View analytics**: Allowed
- **Edit shared Analytics views**: Not allowed
- **Delete shared Analytics views**: Not allowed

#### Test Plans:
- **Create test runs**: Allowed
- **Delete test runs**: Not allowed
- **Manage test configurations**: Not allowed
- **View test runs**: Allowed

#### Other:
- **Manage delivery plans**: Not allowed

![devops-permission](https://github.com/user-attachments/assets/cc0faf18-f6ea-4d58-8cec-4c8073c855f0)



#### QA Group
- Created a group named `CSI-QA-Team`

![qa-team](https://github.com/user-attachments/assets/62f70ab7-25a7-4a37-8f11-1a8a1a0d159e)


- Members: Added Myself as a Tester

![qa-permissions](https://github.com/user-attachments/assets/668d483a-d878-4d98-9462-4d059d4dc97f)


#### Admin Group
- Created a group named `CSI-DevOps-Admin-Team`

![devops-team](https://github.com/user-attachments/assets/b14ec1b8-3953-42b0-877c-dc79d5c3da87)


- Members: Only leads/senior engineers (Added my self again)
- Permissions:
  - **Full Access:** 
  - Includes managing repos, policies, pipelines, boards, and artifacts.

![devops-permission](https://github.com/user-attachments/assets/f8299725-60cf-4418-ab64-40ea2f11067b)


> Each group was created under **Project Settings > Permissions > New Group**, and the necessary permissions were selected appropriately.

---

### Conclusion

In this task, I created a new Azure DevOps project and set up three user groups: Developers, QA, and Admins. Each group was assigned specific permissions tailored to their roles. Developers have limited permissions, QA can manage test runs, and Admins have full control over the project. This setup ensures proper access control and collaboration within the team.

---
