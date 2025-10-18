## Advanced Concept Of Git

### What is stash?

- git stash temporarily saves (stashes) your uncommitted changes — both staged and unstaged — so that you can switch branches or work on something else without committing those changes. -`Example` : You cloned a repo → modified a config file for local use → don’t want to push it → so you “stash” it to hide those changes.
- This sounds logical, but technically git stash is temporary, not meant for permanently ignoring local-only files.
- Once you stash, those changes are removed from your working directory until you reapply them with git stash apply or pop.

- The moment you exec this command `git stash` all the files under `staged` and `unstaged` now remove and added into the `stash` place.
- Both staged and unstaged changes in tracked files are saved in the stash.
- But those which are not added in stage area would not stashed.
- How to see all the stashed files?
- Exec this command :`git stash list`
- Each entry represents one “stash snapshot.”
- how to add again those file into staged area : `git stash apply` but still those files are there in stash area.
- Your files return to the working + staged areas as they were.
- If you wanted to add both tracked and untracked file to add in stash area then exec this command : `git stash -u` .
- You can also `apply` stash on a particular version: `git stash apply stash{version_no}` , otherwise bydefault it will apply on last stashed version.
  -In stash also you can get merge conflic ,how?
- lets suppose you have `apply` on that version and now you commited the changes and again you are doing again `git stash apply stash{same_version}` at this moment git confused , so it will throw merge conflict (bcoz you arer applying stash on the same line).
- You can also do stash only on all untracked files using this command:
  - `git stash --include-tracked`
- How to add only single untracked file.
  - `git stash --include-tracked --<file>`
- How to remove stash?
  - `git stash pop`
- if you just want to remove the stash without adding into staged area:
  - `git stash drop`
- You can also drop stask using version_no:
  - `git stash drop stash@{version}`
- You can remove all the stash without adding into staged area:
  - `git stash clear`
- How to give message while stashing the files:
  - `git stash save "message"`
- For untracked use this cmd:
  - `git stash save "message" --include-untracked`

### One Commit Theory

- The “one commit theory” is more of a best practice philosophy for Git commits rather than a Git command.
- Each commit should represent one logical change or one atomic unit of work.
- A commit should do one thing (e.g., fix a bug, add a feature, or update a config file).

- It should not mix multiple unrelated changes (e.g., bug fix + style changes + new feature in one commit).
- lets suppose you have added all the files in one commit but after that you have made some minore changes in some file and now you want to add this changes in repo so you need to first commit the files in order to add new commit you can add those changes in previous commit , how to do this?
- Use this command :
  - `git commit --ammend`
- This will add those changes in previous commit and you would able to create a new commit(thats what we wanted) , using this we are also able to fulfill the `one commit theory`.
- This commit theory vary company to company , no theory is perfect ,according to you , you can create your own theory.
- If you want to check your ammend then use this command;
  - `git reflog`
  - it will stores all the log wither ammend/commit.
  - so intenally git stores everythings that you are doing.
  - if you ammend the files , commit id should be changed because of timestamp.

### Lets talk about branches concept in git

- Most of the people belive that branches are nothing but a multiple timeline of same things , but that`s not true`, it is actually a `pointer to a commits`.
- When you use this command `git checkout -b <branchname>` it does nothing but init a pointer and shifts that pointer to a new child of that commits , thats why you can able to convert each commits into branch.

  - `git checkout -b <branchname>` does two things:

    - It creates a new branch pointing to the current commit.

    - It switches you to that branch.
    - Because branches are pointers, you can create a branch from any commit, not just the latest one. This is why it seems like “converting a commit into a branch.”

- `Head` : It is also a single pointer to a commit.
  - what it does?
  - It tells us that what brach you are working on and whats the current commit.
- What is a Tag?

  - A tag in Git is a reference (pointer) to a specific commit.

  - Unlike a branch, a tag does not move when you make new commits.

  - Tags are usually used to mark important points in history, like releases (v1.0, v2.0).

```
| Feature             | Tag                        | HEAD                                       |
| ------------------- | -------------------------- | ------------------------------------------ |
| **Type**            | Static pointer to a commit | Dynamic pointer (to branch or commit)      |
| **Moves on commit** | ❌ No                       | ✅ Yes, if pointing to a branch             |
| **Purpose**         | Mark releases, milestones  | Track current working position             |
| **Creation**        | `git tag <name>`           | Automatic, moves as you checkout or commit |
```
