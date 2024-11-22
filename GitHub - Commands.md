# Git commands

## Introduction
GitHub is a platform for storing and sharing code. This guide covers basic GitHub commands to help you manage your code, track changes, and work with others on projects.

---

### Git global auth/credentials
```bash
git config --global user.email "villanuevajoshua27@gmail.com"
git config --global user.name "koykoy027"
```

### Git remote auth/credentials
```bash
git config user.email "villanuevajoshua27@gmail.com"
git config user.name "koykoy027"
```

### Generate SSH
```
ssh-keygen -t rsa -b 4096 -C "villanuevajoshua27@gmail.com"
```

### How to Login via Personal access token
```bash
git credential-store --file ~/.git-credentials store <<EOF
protocol=https
host=github.com
username=koykoy027
password=`your_personal_access_token`
EOF
```

```bash
git config --global credential.helper store
```

### Verify Authentication

To check the global Git user name:
```bash
git config --global user.name
```

To check the global Git user email:
```bash
git config --global user.email
```

To check the list of all configurations, including user-related configurations:
```bash
git config --global --list
```

If you are looking to see which user is currently authenticated for a specific repository, you can use the following command:
```bash
git config user.name
```

If you want to see information about the currently logged-in user on your Ubuntu system, you can use the whoami command:
```bash
whoami
```

### Update git

update default branch, from `master ` to `main`
```bash
git branch -m master main
git fetch origin
git branch -u main main
git remote set-head origin -a
```

If the remote URL or branch name is incorrect, update it using the following commands
```bash
git remote set-url origin <new-remote-url>
git branch --set-upstream-to=origin/<correct-branch-name> main
```

### How to ignore all files and folders
```bash
*
!*gitignore
```

### How to revert commit with SHA
```bash
git revert -m 1 <SHA>
```
