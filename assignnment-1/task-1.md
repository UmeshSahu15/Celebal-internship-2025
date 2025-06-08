# Task 1: File Permissions

## Description
Create a file named `umesh.txt`, assign permissions (read, write, execute) to different user categories (owner, group, others), and practice changing permissions using `chmod`.

## Instructions

### 1. Create the File
```bash
touch umesh.txt
```

![Screenshot 2025-06-07 174532](https://github.com/user-attachments/assets/c0cfcee1-2be2-4b23-b830-e8d573740191)


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
![Screenshot 2025-06-07 181252](https://github.com/user-attachments/assets/b915f1a0-c464-4c0e-b76d-cebaca1dfefe)


### 3. Verify Permissions

Check the permissions with:

```bash
ls -al umesh.txt
```

Expected output example:
```
-rwxr-xr-- 1 umesh umesh 0 Jun 7 17:40 umesh.txt
```
![Screenshot 2025-06-07 181308](https://github.com/user-attachments/assets/00f960af-2d92-4546-bed2-f19107085da1)

### Breakdown of Permission String

- `-` : Regular file
- `rwx` : Owner permissions (read, write, execute)
- `r-x` : Group permissions (read, execute)
- `r--` : Others permissions (read only)

## Resources
- [YouTube: Understanding File Permissions](https://www.youtube.com/watch?v=iwolPf6kN-k)
- [Pluralsight: Linux File Permissions](https://www.pluralsight.com/blog/it-ops/linux-file-permissions)
