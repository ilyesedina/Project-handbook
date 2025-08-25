# Linux & Git Command Reference

A quick reference guide for commonly used Linux and Git commands learned by a migration project. 

---

## 📂 Linux Commands

- **Show hidden files with details in a directory**  
  ```bash
  ls -lha
  ```
- **Move or rename files/directories**
   ```bash
    mv old-repo-name new-repo-name
    ```
 ## 🌱 Git Commands
- **Switch to the latest tag (use Tab to autocomplete)**
    ```bash
    git switch <latest-tag>
    ```
- **Create a new directory and move files**
    ```bash
    mkdir kms-encryption-key
    git mv *.tf kms-encryption-key
    ```
- **Move a file into a subdirectory**
    ```bash
    git mv .terraform-version name-of-subfolder
    ```
- **Stage changes and commit**
    ```bash
    git commit -m "moved files to subdirectory"
    ```
- **Add a new remote (GitHub)**
    ```bash
    git remote add github git@github.com:LEGO/aws-tf-modules-storage.git
    ```
- **List remotes**
    ```bash
    git remote -v
    ```
- **Remove a remote (e.g., origin)**
    ```bash
    git remote remove origin
    ```
- **Push to GitHub remote**
    ```bash
    git push github
    ```
