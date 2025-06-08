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
    
  ![WhatsApp Image 2025-06-08 at 11 20 20_640490ab](https://github.com/user-attachments/assets/083a8f87-86b4-4744-8414-8e1921e29e64)


    - *cd ..*: Move up one directory level.
    
          cd ..
    
   ![WhatsApp Image 2025-06-08 at 11 20 17_c7adb8c3](https://github.com/user-attachments/assets/71946d9d-7407-4c11-9b52-5fb749025dfa)


    - **cd ~**: Change to the home directory.
    
          cd ~
    
   
![WhatsApp Image 2025-06-08 at 11 20 18_0ecf1690](https://github.com/user-attachments/assets/f1b0ffb4-7ba2-4b4a-8005-4afe182fdac3)

3. *Make Directory*
    - *mkdir new_directory*: Create a new directory.
    
          mkdir new_directory
    
   ![WhatsApp Image 2025-06-08 at 11 20 20_a5d9569b](https://github.com/user-attachments/assets/4a3c085b-2021-470a-8254-57b0bad97816)

    - *mkdir -p parent_directory/new_directory*: Create nested directories with a single command.
   
          mkdir -p parent_directory/new_directory
    
  ![WhatsApp Image 2025-06-08 at 11 20 19_9c3f4234](https://github.com/user-attachments/assets/288bdbe9-9908-4bca-a9fc-2da336e660a5)


4. *Remove Directory*
    - *rmdir directory_name*: Remove an empty directory.
  
          rmdir directory_name
  
 ![WhatsApp Image 2025-06-08 at 11 20 19_82952b77](https://github.com/user-attachments/assets/01099ade-4aa8-4e9e-8ce1-a4b82b3f27f9)

    - *rm -r directory_name*: Remove a directory and its contents recursively.
    
          rm -r directory_name
    
  ![WhatsApp Image 2025-06-08 at 11 20 19_956e0bb1](https://github.com/user-attachments/assets/468f797e-7e72-46e1-a61f-b43dcbe632fa)

5. *Create a File*
    - *touch newfile.txt*: Create a new, empty file.
    
          touch newfile.txt
    
  ![WhatsApp Image 2025-06-08 at 11 20 20_aca3ed32](https://github.com/user-attachments/assets/9c43e2aa-b47a-433b-85b7-96bdbcc87316)

6. *Remove a File*
    - *rm filename*: Remove a file.
  
          rm filename
    ![Screenshot 2025-06-07 193135](https://github.com/user-attachments/assets/ac79f96f-fb56-460d-a09b-0e15691904e6)

    
7. *Move/Rename a File or Directory*
    - *mv old_name new_name*: Rename a file or directory.
   
          mv old_name new_name
    
   ![WhatsApp Image 2025-06-08 at 11 20 20_5af6ad09](https://github.com/user-attachments/assets/dcfa5b63-1e74-4aea-bc13-297a17039de3)

    - *mv filename /path/to/destination*: Move a file to a different directory.
   
          mv filename /path/to/destination
    
    ![Screenshot (42)](https://github.com/manish-g0u74m/celebaltech-inturn/assets/148465299/7c596138-dbc7-4217-8ae1-c263923cbaa9)

8. *Copy a File or Directory*
    - *cp filename /path/to/destination*: Copy a file to a different directory.
  
          cp filename /path/to/destination
    
![image](https://github.com/user-attachments/assets/aded57de-21df-4a82-8248-55df198ae43c)

    - *cp -r source_directory /path/to/destination*: Copy a directory and its contents recursively.
       cp -r source_directory /path/to/destination
    
![image](https://github.com/user-attachments/assets/53a51601-6992-482c-b351-0f7014c957d7)


10. *Display File Contents*
    - *cat > filename*: Add Content in a file.
  
          cat > filename
    
  ![WhatsApp Image 2025-06-08 at 11 20 21_09b4200a](https://github.com/user-attachments/assets/401c5a99-47b8-4bb7-98c0-436254726482)

     - *cat filename*: Display the contents of a file.
    
           cat filename
    
![WhatsApp Image 2025-06-08 at 11 20 22_786779fa](https://github.com/user-attachments/assets/40958291-cd9a-427d-897b-bc87120a966d)



11. *Print Working Directory*
    - *pwd*: Display the current directory.
    
          pwd
 
![WhatsApp Image 2025-06-08 at 11 20 23_f08f2400](https://github.com/user-attachments/assets/8fe0d054-0dad-40ed-9fe1-e9e36fbc02a3)

   

## Resources
- [Red Hat: Create and Delete Files and Directories in Linux](https://www.redhat.com/sysadmin/create-delete-files-directories-linux)
