
# Assignment Submission Guide Using Git

## 📌 Introduction to Version Control and Git
Version control is a system that records changes to files over time. Git is a distributed version control system used to track code changes, collaborate with others, and manage software projects efficiently.

---

## 🛠 Git Installation and Configuration

### Install Git:
- Download Git from the official website: [https://git-scm.com/](https://git-scm.com/)
- Follow the installation instructions for your operating system.

### Configure Git (run in terminal):
```bash
git config --global user.name "Umesh-sahu"
git config --global user.email "umeshkumar637825@gmail.com"


```

## 💡 Basic Git Commands

### Initialize a Git Repository
```bash
git init

```
### Add Files to Staging Area
```bash
git add .           # Adds all files
git add week1    # Adds specific file
```

### Commit Changes
```bash
git commit -m "Assignment-1 Complete"
```

### Connect to Remote Repository
```bash
git remote add origin [https://github.com/UmeshSahu15/CSI-internship-2025.git]
```

### Push Changes to Remote Repository
```bash
git push -u origin main
```

### Pull Latest Changes from Remote Repository
```bash
git pull origin main


```

## ✅ Assignment Submission Workflow

1. *Clone Repository* (if applicable):[https://github.com/UmeshSahu15/CSI-internship-2025.git]
   
       git clone 
   

2. *Make Changes* to the assignment files.

3. *Stage and Commit* your changes:

       git add .
       git commit -m "Completed assignment task "
   

4. *Push* your changes:
  
       git push origin main
   

5. *Verify* your submission by checking the remote repository.
