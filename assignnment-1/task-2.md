# Task 2: Execute Basic Linux Commands

## Descripton
Execute basic Linux commands to manipulate files and directories, with an emphasis on understanding their usage. This task includes listing files, creating and deleting directories, changing directories, and more.

## Instructions and Commands

1. *List Files and Directories*
 - *ls*: List the files and directories in the current directory.
    
          ls
    
 ![WhatsApp Image 2025-06-08 at 11 20 18_dd690348](https://github.com/user-attachments/assets/cdf9dab2-7ff4-4f5b-85a6-003fa73d6e35)

  - *ls -l*: List files and directories with detailed information.
    
          ls -l
    
  ![WhatsApp Image 2025-06-08 at 11 20 17_3367430e](https://github.com/user-attachments/assets/fb33e34c-0ec4-44f2-ba1e-453a0436f347)


  - *ls -a*: List all files, including hidden files (those starting with a dot).
    
          ls -a
    
    ![WhatsApp Image 2025-06-08 at 11 20 17_5d2eec9b](https://github.com/user-attachments/assets/7c61fd79-eb1b-4757-b947-a6d5034a7fa9)


    - *ls -la*: Combine detailed and hidden file listing.
    
          ls -la
    
  ![WhatsApp Image 2025-06-08 at 11 20 18_75fbb796](https://github.com/user-attachments/assets/4bbf54a6-381d-424d-a07a-3596e7de3e47)

2. *Change Directory*
    - *cd directory_name*: Change to the specified directory.
    
          cd directory_name
    
   ![Screenshot 2025-06-07 192443](https://github.com/user-attachments/assets/9e1cd6a7-23da-4e13-ad7d-c95976ad90ba)

    - *cd ..*: Move up one directory level.
    
          cd ..
    
    ![Screenshot (32)](https://github.com/manish-g0u74m/celebaltech-inturn/assets/148465299/0c7c9c33-d5d2-4cda-8f41-ba413248a6b7)

    - **cd ~**: Change to the home directory.
    
          cd ~
    
   
![Screenshot 2025-06-07 192634](https://github.com/user-attachments/assets/f2e93f87-586c-4e2e-ab4f-1941d66e9c3e)

3. *Make Directory*
    - *mkdir new_directory*: Create a new directory.
    
          mkdir new_directory
    
    ![Screenshot 2025-06-07 192655](https://github.com/user-attachments/assets/b8b9b929-4e75-429d-87c5-b092b7a69788)

    - *mkdir -p parent_directory/new_directory*: Create nested directories with a single command.
   
          mkdir -p parent_directory/new_directory
    
   ![Screenshot 2025-06-07 192807](https://github.com/user-attachments/assets/d62cd253-8ad4-4943-b93d-c125de53a86c)

4. *Remove Directory*
    - *rmdir directory_name*: Remove an empty directory.
  
          rmdir directory_name
  
  ![Screenshot 2025-06-07 192945](https://github.com/user-attachments/assets/743668d6-403a-411c-a8fd-f2e2ce373b51)

    - *rm -r directory_name*: Remove a directory and its contents recursively.
    
          rm -r directory_name
    
   ![Screenshot 2025-06-07 193004](https://github.com/user-attachments/assets/bb4dbc57-314c-473d-aa84-866c507e2659)

5. *Create a File*
    - *touch newfile.txt*: Create a new, empty file.
    
          touch newfile.txt
    
   ![Screenshot 2025-06-07 174532](https://github.com/user-attachments/assets/12054832-aeea-4b5a-9075-b2979980dd4b)

6. *Remove a File*
    - *rm filename*: Remove a file.
  
          rm filename
    ![Screenshot 2025-06-07 193135](https://github.com/user-attachments/assets/ac79f96f-fb56-460d-a09b-0e15691904e6)

    
7. *Move/Rename a File or Directory*
    - *mv old_name new_name*: Rename a file or directory.
   
          mv old_name new_name
    
    ![Screenshot 2025-06-07 193122](https://github.com/user-attachments/assets/aa3be6f4-841f-4176-9326-86594aa956c0)

    - *mv filename /path/to/destination*: Move a file to a different directory.
   
          mv filename /path/to/destination
    
    ![Screenshot (42)](https://github.com/manish-g0u74m/celebaltech-inturn/assets/148465299/7c596138-dbc7-4217-8ae1-c263923cbaa9)

8. *Copy a File or Directory*
    - *cp filename /path/to/destination*: Copy a file to a different directory.
  
          cp filename /path/to/destination
    
   ![Screenshot 2025-06-07 193240](https://github.com/user-attachments/assets/b841eb86-3d75-486c-86b0-f11b22647169)

    - *cp -r source_directory /path/to/destination*: Copy a directory and its contents recursively.
   
          cp -r source_directory /path/to/destination
    
    ![Screenshot (44)](https://github.com/manish-g0u74m/celebaltech-inturn/assets/148465299/7c506789-0459-4696-a9a2-c77f805b2826)

10. *Display File Contents*
    - *cat > filename*: Add Content in a file.
  
          cat > filename
    
    ![Screenshot 2025-06-07 193404](https://github.com/user-attachments/assets/9ac23fad-2dd6-451e-9ba9-6b416dfec2d9)

     - *cat filename*: Display the contents of a file.
    
           cat filename
    
 ![Screenshot 2025-06-07 193429](https://github.com/user-attachments/assets/e340cb03-6e72-412f-9489-90fdfd743f48)


11. *Print Working Directory*
    - *pwd*: Display the current directory.
    
          pwd
 
 ![Screenshot 2025-06-07 193439](https://github.com/user-attachments/assets/ef9c5af9-7f4e-40d7-8a1c-8c653561233a)
   

## Resources
- [Red Hat: Create and Delete Files and Directories in Linux](https://www.redhat.com/sysadmin/create-delete-files-directories-linux)
