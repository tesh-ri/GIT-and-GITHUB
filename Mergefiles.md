## Lets talk about git merge and git rebase

### Git Rebase:

- git rebase is the process of moving or “replaying” commits from one branch onto another branch, creating a linear history.
- In simpler words:
- It takes your commits and puts them on top of another branch as if they were made after it.
- `Example`:
  - You have a commit history like this:

```
A -- B -- C  (main)
      \
       D -- E  (feature)
```

- If you run this :
  1.  `git checkout feature`
  2.  `git rebase main`
- Git will replay D and E on top of C:

```
A -- B -- C -- D' -- E'  (feature)
```

- D' and E' are new commits (different hashes)

- feature branch is now linear on top of main

- There’s no merge commit
- How it works ?

  - Git finds the common ancestor of the two branches.

  - Temporarily removes your commits (on feature branch).

  - Applies them one by one on top of the target branch.

  - Resolves conflicts if any arise during the replay.

---

### Lets talk about `git merge`

- git merge combines two branches by creating a new “merge commit” that has two parents.

- Example:

```
A -- B -- C  (main)
      \
       D -- E  (feature)
```

- `git checkout main`
- `git merge feature`
- Results in :

```
A -- B -- C ------- M (main)
      \           /
       D -- E (feature)

```

- M is a merge commit

- History preserves branch structure

- No commits are rewritten

- ## There is one more concept Fast-Forward-Merge:

  - A fast-forward merge happens when the branch you want to merge has all its commits ahead of the current branch, and there are no new commits on the current branch since the divergence.
  - In simpler terms:
    - Git can just move the HEAD pointer forward to the latest commit.
    - No new merge commit is created.

## What are the best way to undo the latest commit

- using `git revert`
- What is `git revert` ?

  - Instead of removing the commit, revert creates a new commit that undoes it.
  - means simply it creates a new commit with those changes that are presnt in previous commit , it does not move the head pointer to the previous one it move the head pointer to the new inverse created commit.
  - `git revert <commitId>`
  - `git revert head` it is use to move back to the 1st previous commit.
  - result :

    - New commit created: Revert "Add feature"

    - Changes from the original commit are undone in code

    - History remains linear and safe

  - When to use :

    - Commit is already pushed to remote

    - You want a safe, non-destructive undo

- While doing the git revert sometimes you will get merge conflict , so you need to resolved that conflict.
- then do git add <files>
- you can abort the gt revert by exec this command : `git revert --abort `
- if you are reverting from 1st head to 3rd head internally you need to resolve two conflcit , if you wanted to skip that patch then you can exec this command `git revert --skip`
- Git skips the current commit and moves on to the next one in the revert sequence.

- The skipped commit is not reverted, but the revert process continues for subsequent commits.

---

## Lets talk about cherry-picking concept

- git cherry-pick is the process of taking a commit from one branch and applying it onto another branch, without merging the entire branch.
- In simpler words:

  - You can select specific commits from another branch and “copy” them onto your current branch.

  - Only the commit’s changes are applied, not the full branch history.

- Example:
  - ```
      main:     A -- B -- C
      feature:       D -- E
    ```
- You are on main and you want only commit D from feature:
- ```
    git checkout main
    git cherry-pick <hash-of-D>

  ```

- Result:
  - ```
    main: A -- B -- C -- D'
    feature:       D -- E
    ```
- D' is a new commit on main with the same changes as D.

- feature branch remains untouched.

#### Cherry-Picking Multiple Commits

- You can cherry-pick a range:
- `git cherry-pick A..B`
- Applies commits from A (exclusive) to B (inclusive)

- Git will replay commits one by one and may pause for conflicts.

- To continue after resolving conflicts:
  - `git cherry-pick --continue`
- To skip a commit during conflicts:
  - ` git cherry-pick --skip`
- To abort the cherry-pick entirely:
  - `git cherry-pick --abort`

---

### Difference between Git pull vs Git fetch

- `git fetch` downloads commits, files, and references from the remote repository but does not merge them into your current branch.

  - Updates your remote tracking branches (like origin/main)

  - Your local branch stays unchanged until you explicitly merge or rebase

  - `git fetch origin`

    - Downloads all new commits from origin

    - Your local branch (say main) is still the same

    - To update your branch manually:

  - Use case: Check what changed on remote without affecting your working directory.

- `git pull `

  - Downloads commits from remote

  - Immediately merges them into your current branch (or rebases if --rebase is used)
  - git pull = git fetch + git merge (by default)
  - `git pull origin main`
  - Fetches latest commits from origin/main

  - Merges them into your current branch automatically
  - Use case: Quickly update your branch with remote changes

- Key differnce:

```
| Feature              | `git fetch`                          | `git pull`                        |
| -------------------- | ----------------------------------   |---------------------------------- |
| Download from remote | ✅                                   | ✅                                |
| Update local branch  |  (branch stays unchanged)            | ✅ (merge or rebase automatically)|
| Safety               | Safer — no automatic merge           |Can lead to conflicts automatically|
| Use case             | Inspect remote changes, plan merge   | Quickly sync branch with remote   |

```
