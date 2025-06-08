# Task 5: Practice More Linux Commands

## Description
Practice more Linux commands to enhance our command line proficiency. This task includes various commands for file and directory manipulation, system information, network management, and more.

## Instructions and Commands

1. *System Information*
    - *uname -a*: Display all system information.
   
          uname -a
    
    - *hostname*: Show or set the system's hostname.
    
          hostname
    
    - *uptime*: Show how long the system has been running, along with the load average.
    
          uptime
    

2. *Disk Usage and Free Space*
    - *df -h*: Display disk space usage in a human-readable format.
    
          df -h
    
    - *du -sh /path/to/directory*: Show the total disk usage of a directory.
    
          du -sh /path/to/directory
    

3. *File Operations*
    - *file filename*: Determine the type of a file.
    
          file filename
    
    - *stat filename*: Display detailed information about a file or filesystem.
    
          stat filename
    

4. *Searching and Finding Files*
    - *find /path -name filename*: Find files by name.
    
          find /path -name filename
    
    - *grep "search_term" filename*: Search for a term within a file.
    
          grep "search_term" filename
    
    - *grep -r "search_term" /path/to/directory*: Recursively search for a term in all files within a directory.
    
          grep -r "search_term" /path/to/directory
    

5. *Process Management*
    - *ps aux*: Display all running processes.
    
          ps aux
    
    - *top*: Display and update sorted information about system processes.
    
          top
    
    - *kill PID*: Kill a process by its Process ID (PID).
    
          kill PID
    

6. *Network Management*
    - *ifconfig*: Display or configure a network interface (older systems).
    
          ifconfig
    
    - *ip addr show*: Display IP addresses and interfaces (newer systems).
    
          ip addr show
    
    - *ping hostname*: Send ICMP ECHO_REQUEST to network hosts.
    
          ping hostname
    
    - *netstat -tuln*: Display network connections, routing tables, interface statistics, masquerade connections, and multicast memberships.
    
          netstat -tuln
    

7. *Compression and Archiving*
    - *tar -czvf archive.tar.gz /path/to/directory*: Create a compressed archive of a directory.
    
          tar -czvf archive.tar.gz /path/to/directory
    
    - *tar -xzvf archive.tar.gz*: Extract a compressed archive.
      
          tar -xzvf archive.tar.gz
    
    - *zip -r archive.zip /path/to/directory*: Create a zip archive of a directory.
      
          zip -r archive.zip /path/to/directory
    
    - *unzip archive.zip*: Extract a zip archive.
    
          unzip archive.zip
    

8. *Text Manipulation*
    - *echo "text" > file.txt*: Create a file with the specified text.
    
          echo "text" > file.txt
    
    - *cat file.txt*: Display the contents of a file.
    
          cat file.txt
    
    - *sort file.txt*: Sort the contents of a file.
    
          sort file.txt
    
    - *wc -l file.txt*: Count the number of lines in a file.
    
          wc -l file.txt
    

9. *Permission Management*
    - *chmod 644 file.txt*: Set read and write permissions for the owner, and read-only permissions for the group and others.
    
          chmod 644 file.txt
    
    - *chown user:group file.txt*: Change the owner and group of a file.
    
          chown user:group file.txt
    

10. *Miscellaneous Commands*
    - *history*: Display the command history.
    
          history
    
    - *alias ll='ls -la'*: Create an alias for a command.
    
          alias ll='ls -la'
    
    - *date*: Display or set the system date and time.
    
          date
    

## Resources
- [Linux Command Library](https://linuxcommandlibrary.com)
