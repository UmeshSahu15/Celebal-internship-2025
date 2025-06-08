# Task 4: User and Group Management

## Description
Create a new user and group, set their permissions, and explore user management commands like useradd, usermod, and userdel.

## Instructions
1. *Create a New User*
    
       sudo useradd newuser
  
  ![Screenshot 2025-06-07 235115](https://github.com/user-attachments/assets/d72087fe-1a1e-47bb-95fd-ac89c12d9666)
  
   
3. *Create a New Group*
    
       sudo groupadd newgroup

   ![Screenshot 2025-06-07 235408](https://github.com/user-attachments/assets/50164f4b-d712-4543-8248-a8c1b391d5fd)

5. *Add User to Group*
    
       sudo usermod -aG newgroup newuser
    

6. *Set Permissions for User and Group*
    
       sudo chmod 750 /path/to/directory
    
   ![Screenshot 2025-06-07 235709](https://github.com/user-attachments/assets/5b6ec589-7326-4f39-a4b9-bdbca7449e39)

8. *Delete a User*
    sh
    sudo userdel newuser
    
    ![Screenshot 2025-06-08 000215](https://github.com/user-attachments/assets/8faee530-4c07-4593-8cd5-a607a6d9160c)

## Resources
- [Red Hat: Manage Permissions](https://www.redhat.com/sysadmin/manage-permissions)
