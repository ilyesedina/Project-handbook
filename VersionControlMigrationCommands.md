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
 ## Migrate files into a new subdirectory structure
   ```bash
    mkdir new-folder
    git mv *.tf new-folder/
    git commit -m "Reorganized repo structure"
    git push
   ```
--------------------
## Unstructured Notes

Create new repo

add contributors to repos:

DCI

Data Platform

Clone the GitLab repo

Latest tag → the tag points to a branch

git checkout (tab) look up the lates tag

git fetch --all

git branch -r

git branch -vv

git branch github_migration

rm -r -f aws-tf-modules-storage (for delectated unwanted files)

git tag v1.0

git tag v1.0 -f

git push origin tag v1.0 -f

git cherry-pick commit-idf22089f250118087a9c019d815cfce

grep -r gitlab

git clean -f -d
