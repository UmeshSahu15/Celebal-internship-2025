# Task 1: File Permissions

## Description
Create a file named `umesh.txt`, assign permissions (read, write, execute) to different user categories (owner, group, others), and practice changing permissions using `chmod`.

## Instructions

### 1. Create the File
```bash
touch umesh.txt
```

![WhatsApp Image 2025-06-08 at 11 20 16_92d2043c](https://github.com/user-attachments/assets/dfa7ea6a-cf0b-48fa-904c-c4af543d73be)




### 2. Assign Permissions

Set permissions as follows:

| User Category | Permissions            | Numeric Value |
|---------------|------------------------|---------------|
| Owner (User)  | Read, Write, Execute   | 7 (4+2+1)     |
| Group         | Read, Execute          | 5 (4+0+1)     |
| Others        | Read only              | 4 (4+0+0)     |

Apply these permissions using:

```bash
chmod 754 umesh.txt
```
![WhatsApp Image 2025-06-08 at 11 20 16_f4f5e257](https://github.com/user-attachments/assets/bd8e6519-f847-420f-a499-c247b569f289)


### 3. Verify Permissions

Check the permissions with:

```bash
ls -al umesh.txt
```

Expected output example:
```
-rwxr-xr-- 1 umesh umesh 0 Jun 7 17:40 umesh.txt
```
![WhatsApp Image 2025-06-08 at 11 20 17_aefdcecb](https://github.com/user-attachments/assets/346db8e9-e30e-4ecc-9aa1-1f26084f55b0)

### Breakdown of Permission String

- `-` : Regular file
- `rwx` : Owner permissions (read, write, execute)
- `r-x` : Group permissions (read, execute)
- `r--` : Others permissions (read only)

## Resources
- [YouTube: Understanding File Permissions](https://www.youtube.com/watch?v=iwolPf6kN-k)
- [Pluralsight: Linux File Permissions](https://www.pluralsight.com/blog/it-ops/linux-file-permissions)
