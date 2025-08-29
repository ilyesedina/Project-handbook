# Linux & Git Command Reference

A quick reference guide for commonly used Linux and Git commands learned by a migration project. 

---

## 📂 Linux Commands

- **Show hidden files with details in a directory**  
  ```bash
  ls -lha
  ```
- **New Directory**
   ```bash
    mkdir new-folder
    ```
- **Move or rename files/directories**
   ```bash
    mv old-repo-name new-repo-name
    ```

   
 ## 🔖 Git Tags
- **List tags**
    ```bash
    git tag -l "v1.8.5*"
    ```
- **Show to which commit the tag attached**
    ```bash
    $ git show v1.4
    commit ca82a6dff817ec66f44342007202690a93763949
    Author: Edina Ilyes <ilyesedina10@gmail.com>
    Date:   Mon Mar 17 21:52:11 2025 -0700
    ```
- **Create a tag**
    ```bash
    $ git tag v1.4-lw
    $ git tag
    v0.1
    v1.4-lw
    v1.5
    ```
- **Switch to the latest tag (use Tab to autocomplete)**
    ```bash
    git switch <latest-tag>
    ```
    
## 🌱 Repo Setup
- **Create new repo**
- **Clone the GitLab repo**
    ```bash
    git clone <repo-url>
    ```
## 🌿 Git Branching
- **List remote branches**
    ```bash
    git branch -r
    ```
- **Verbose branch info**
    ```bash
    git branch -vv
    ```
- **Create migration branch**
    ```bash
    git branch github_migration
    ```

## 🔄 Fetch & Sync
- **Fetch all branches and tags**
    ```bash
    git fetch --all
    ```
- **Fetch from local repo**
    ```bash
    git fetch ../iam_policy_s3_access
    ```

## 🌐 Git Remotes
- **Add a new remote (GitHub)**
    ```bash
    git remote add <remote name> <remote path on github>
    git remote add github git@github.com:LEGO/aws-tf-modules-storage.git
    ```
- **Add a local repo as remote**
    ```bash
    git remote add -f iam-azuread-role ../iam-azuread-role
    ```
- **List remotes**
    ```bash
    git remote -v
    ```
- **Push to specific remote**
    ```bash
    git push github
    git push <remote-name>
    ```
- **Remove a remote (e.g., origin)**
    ```bash
    git remote remove origin
    ```

 ## 📂 File & Directory Management
- **Create a new directory and move files into a new subdirectory**
    ```bash
    mkdir kms-encryption-key
    git mv *.tf kms-encryption-key
    ```
- **Move a file into a subdirectory**
    ```bash
    git mv .terraform-version name-of-subfolder
    ```
- **Remove unwanted directory**
    ```bash
    rm -rf aws-tf-modules-storage
    ```

----------------------------
## ✅ Staging & Committing

- **Stage changes and commit**
    ```bash
    git add -A
    git commit -m "moved files to subdirectory"
    ```
- **Push to GitHub remote**
    ```bash
    git push github
    ```
    
## 🛠️ Migration Workflow Example
   ```bash
    mkdir new-folder
    git mv *.tf new-folder/
    git commit -m "Reorganized repo structure"
    git push
   ```
 ## 🌉 Merging & Cherry-Picking
 - **Merge repos (allow unrelated histories)**
   ```bash
    git merge aws-tf-modules-iam/master --no-commit --allow-unrelated-histories
    git merge iam-azuread-role/github_migration --no-commit --allow-unrelated-histories
   ```
 - **Cherry-pick a commit**
   ```bash
   git cherry-pick <commit-id>
   ```
   
## 📜 Logs & Inspection
 - **View commit history**
   ```bash
    git log
   ```
 - **Search repo for references**
   ```bash
    grep -r gitlab
   ```
