# Git_Tutorial
Author:- Maharshi Patel

## ✨ Core Git Concepts: What They Are & How to Use Them

This section details essential Git commands and concepts.

### 1. `git clone`

* **What it is:** Used to create a local copy of an existing Git repository from a remote source. It downloads all files, branches, and the entire commit history.
* **How to use:**
    ```bash
    git clone <repository_url> [destination_directory]
    ```

### 2. Local Repository / Remote Repository

* **Local Repository:** The copy of the Git repository that resides on your own computer, containing all project files, history, and branches.
* **Remote Repository:** A version of the repository hosted on a server (e.g., GitHub), serving as a central point for collaboration and backup.

### 3. `git pull`

* **What it is:** Used to fetch changes from a remote repository and integrate (merge) them into your current local branch. It combines `git fetch` and `git merge`.
* **How to use:**
    ```bash
    git pull [remote_name] [branch_name]
    ```

### 4. `git push`

* **What it is:** Used to upload your local commits to a remote repository, making your local changes available to others and updating the remote's history.
* **How to use:**
    ```bash
    git push [remote_name] [branch_name]
    ```
    * For the first push of a new local branch: `git push -u origin <branch_name>`

### 5. `git commit`

* **What it is:** Used to record changes to the repository as a snapshot of your project at a specific time.
* **How to use:**
    * First, stage your changes: `git add .` or `git add <file_name>`
    * Then, commit:
        ```bash
        git commit -m "Your descriptive commit message"
        ```

### 6. `git merge`

* **What it is:** Used to combine changes from one branch into another, integrating the source branch's history into the target branch.
* **How to use:**
    1.  Switch to the target branch:
        ```bash
        git checkout <target_branch>
        ```
    2.  Perform the merge:
        ```bash
        git merge <source_branch>
        ```

### 7. Branch Creation and Branch Merging

* **Branch Creation:**
    * **What it is:** Creating a separate line of development to work on new features or fixes in isolation.
    * **How to use:**
        * Create new branch (stay on current):
            ```bash
            git branch <new_branch_name>
            ```
        * Create and switch to new branch:
            ```bash
            git checkout -b <new_branch_name>
            ```
* **Branch Merging:** (Refer to `git merge` section above)

### 8. `git stash`

* **What it is:** Temporarily saves changes that you don't want to commit immediately, allowing you to switch contexts without losing work.
* **How to use:**
    1.  **Stash changes:**
        ```bash
        git stash [save "Optional message"]
        ```
    2.  **View stashes:**
        ```bash
        git stash list
        ```
    3.  **Apply stashed changes:**
        ```bash
        git stash apply [stash@{n}] # Applies a specific stash, keeps it in list
        git stash pop             # Applies most recent stash, removes it from list
        ```
    4.  **Clear/Drop stashes:**
        ```bash
        git stash clear           # Removes all stashes
        git stash drop [stash@{n}] # Removes a specific stash
        ```

### 9. `git revert`

* **What it is:** Undoes a specific commit by creating a *new commit* that reverses the target commit's changes. It maintains history integrity.
* **How to use:**
    ```bash
    git revert <commit_hash>
    ```

### 10. `git reset`

* **What it is:** A powerful command to undo local changes by moving the `HEAD` pointer to a specified commit, essentially rewriting history *locally*.
* **How to use:**
    ```bash
    git reset [--soft | --mixed | --hard] <target_commit_ish>
    ```
    * `--soft`: Moves `HEAD`, keeps changes staged.
    * `--mixed` (default): Moves `HEAD`, unstages changes.
    * `--hard`: Moves `HEAD`, discards all changes. (Use with caution!)

### 11. `git restore`

* **What it is:** Used to undo local modifications to files, either in the working directory (unstaged changes) or in the staging area (staged changes). It provides a clearer way to discard changes compared to older methods.
* **How to use:**
    * To discard unstaged changes in the working directory:
        ```bash
        git restore <file_name>
        ```
    * To unstage changes (move from staging to working directory):
        ```bash
        git restore --staged <file_name>
        ```
    * To restore a file to its state at a specific commit:
        ```bash
        git restore --source <commit_hash> <file_name>
        ```

### 12. `git rebase`

* **What it is:** Used to move or combine a sequence of commits to a new base commit. It rewrites commit history by re-applying commits from one branch on top of another, often used to create a clean, linear history.
* **How to use:**
    * To rebase your current branch onto another branch (integrating changes cleanly):
        ```bash
        git rebase <base_branch_name>
        ```
    * To perform an interactive rebase (e.g., to squash, edit, or reorder commits):
        ```bash
        git rebase -i <commit_ish>
        # Example: git rebase -i HEAD~3 (for the last 3 commits)
        # Example: git rebase -i main (for commits on current branch not on main)
        ```

---
## 🚀 Typical Git Workflow

This section outlines a common development workflow using the Git commands explained above. This flow helps maintain a clean, collaborative, and organized project history.

1.  **Start a New Feature/Fix:**
    * First, ensure your `main` (or `master`) branch is up-to-date:
        ```bash
        git checkout main
        git pull origin main
        ```
    * Create a new branch for your work:
        ```bash
        git checkout -b feature/your-feature-name
        ```

2.  **Make Changes & Commit Locally:**
    * Modify, add, or delete files as needed for your feature.
    * Stage your changes:
        ```bash
        git add .
        ```
    * Commit your changes with a descriptive message:
        ```bash
        git commit -m "feat: implement X functionality"
        ```
    * Repeat this step for logical units of work.

3.  **Handle Interruptions (Optional):**
    * If you need to switch branches urgently without committing your work:
        ```bash
        git stash
        # ... do other work ...
        git stash pop # To bring back your changes
        ```

4.  **Keep Your Branch Updated (Optional, but Recommended):**
    * Before pushing or creating a Pull Request, you might want to bring in the latest changes from `main` to avoid large merge conflicts later.
        ```bash
        git fetch origin
        git rebase origin/main # Or git merge origin/main
        ```

5.  **Share Your Work (Push to Remote):**
    * Push your feature branch to the remote repository:
        ```bash
        git push -u origin feature/your-feature-name
        ```

6.  **Merge into Main (via Pull Request):**
    * Typically, you'd open a **Pull Request (PR)** or **Merge Request (MR)** on your Git hosting platform (GitHub, GitLab, Bitbucket).
    * After review and approval, the branch is merged into `main`. The underlying Git operation is `git merge`.

7.  **Clean Up:**
    * After your feature branch is successfully merged into `main` (and often after the PR is closed), you can delete the local and remote feature branches:
        ```bash
        git checkout main                     # Switch back to main
        git pull origin main                  # Get the latest changes (including your merged feature)
        git branch -d feature/your-feature-name # Delete local branch
        git push origin --delete feature/your-feature-name # Delete remote branch
        ```

8.  **Undoing Changes (If Needed):**
    * **Revert a bad commit that's already pushed:**
        ```bash
        git revert <bad_commit_hash>
        git push origin main
        ```
    * **Discard local uncommitted changes:**
        ```bash
        git restore . # Discard all unstaged changes
        ```
    * **Undo local commits not yet pushed (use with caution):**
        ```bash
        git reset --soft HEAD~1 # Undo last commit, keep changes staged
        git reset --hard HEAD~1 # Undo last commit, discard all changes
        ```

---