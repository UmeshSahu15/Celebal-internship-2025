# Task 4: User and Group Management

## Description
Create a new user and group, set their permissions, and explore user management commands like useradd, usermod, and userdel.

## Instructions
1. *Create a New User*
    
       sudo useradd newuser
  
![WhatsApp Image 2025-06-08 at 11 20 22_0926cf7e](https://github.com/user-attachments/assets/9b34b8b9-afd6-4c3d-ae0f-1e40fd377b66)

   
3. *Create a New Group*
    
       sudo groupadd newgroup

  ![WhatsApp Image 2025-06-08 at 11 20 23_51a054f9](https://github.com/user-attachments/assets/6cd027f6-8d98-4ec8-9b8c-7dab8c143e42)

5. *Add User to Group*
    
       sudo usermod -aG newgroup newuser
    

6. *Set Permissions for User and Group*
    
       sudo chmod 750 /path/to/directory
    
  ![WhatsApp Image 2025-06-08 at 11 20 23_d7fec536](https://github.com/user-attachments/assets/5f335910-9119-4444-87bc-87cea938c09a)

8. *Delete a User*
    sh
    sudo userdel newuser
    
   ![image](https://github.com/user-attachments/assets/de44f735-bbeb-49e7-9849-5d1583cec052)


## Resources
- [Red Hat: Manage Permissions](https://www.redhat.com/sysadmin/manage-permissions)
