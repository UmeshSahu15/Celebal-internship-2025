
# Task 3: Navigating Directories

## Description
Using the terminal, practice navigating through directories, listing file contents, and moving files to different locations. This task helps to build proficiency in managing file systems through the command line.

## Instructions and Commands

1. *Change Directory*
    - *cd directory_name*: Change to the specified directory.
  
          cd directory_name
    
    
    - *cd ..*: Move up one directory level.
      
          cd ..
    
    

    - **cd ~**: Change to the home directory.
    
          cd ~
    
    
    - *cd /path/to/directory*: Change to an absolute path.
      
          cd /path/to/directory
    
3. *List Directory Contents*
    - *ls*: List the files and directories in the current directory.
   
          ls
    
   
    - *ls -l*: List files and directories with detailed information including permissions, number of links, owner, group, size, and modification date.
    
          ls -l
    
   
    - *ls -a*: List all files, including hidden files (those starting with a dot).
    
          ls -a
    
    
    - *ls -la*: Combine detailed and hidden file listing.
    
          ls -la
    
    
    - *ls -lh*: List files with human-readable file sizes.
    
          ls -lh
    
5. *Move Files*
    - *mv old_name new_name*: Rename a file or directory.
    
          mv old_name new_name
    
    - *mv filename /path/to/destination*: Move a file to a different directory.
    
          mv filename /path/to/destination
    
  
    - *mv source_directory /path/to/destination*: Move a directory to a different location.
    
          mv source_directory /path/to/destination
    
   
7. *Copy Files*
    - *cp filename /path/to/destination*: Copy a file to a different directory.
    
          cp filename /path/to/destination
    
    
    - *cp -r source_directory /path/to/destination*: Copy a directory and its contents recursively.
    
          cp -r source_directory /path/to/destination
    
    
9. *Create Files and Directories*
    - *touch newfile.txt*: Create a new, empty file.
    
          touch newfile.txt
    
   
    - *mkdir new_directory*: Create a new directory.
    
          mkdir new_directory
    
    
10. *Remove Files and Directories*
    - *rm filename*: Remove a file.
    
          rm filename
    
   
    - *rm -r directory_name*: Remove a directory and its contents recursively.
    
          rm -r directory_name
    
    
11. *Display File Contents*
    - *cat filename*: Display the contents of a file.
    
          cat filename
    
    
    - *head filename*: Display the first 10 lines of a file.
    
          head filename
    
   
    - *tail filename*: Display the last 10 lines of a file.
      
          tail filename
   
  ![Screenshot 2025-06-08 114157](https://github.com/user-attachments/assets/fbec5a20-244d-4199-ba6e-d22d68ce628d)


12. *Print Working Directory*
    - *pwd*: Display the current directory.
    
          pwd
    

13. *Find Files*
    - *find /path/to/search -name filename*: Search for a file by name within a directory.
    
          find /path/to/search -name filename
   ![image](https://github.com/user-attachments/assets/29262139-072d-4b23-b7d3-ad1c063776e4)

14. *Check Disk Usage*
    - *du -h /path/to/directory*: Display the disk usage of a directory and its contents in a human-readable format.
    
          du -h /path/to/directory
    
   ![Screenshot 2025-06-08 114423](https://github.com/user-attachments/assets/af722e4d-4928-448d-b4b8-e5df044580f6)


## Resources
- [Red Hat: Navigating the Filesystem in Linux Terminal](https://www.redhat.com/sysadmin/navigating-filesystem-linux-terminal)
