## Lets talk about `git reset`:

- `git checkout <commit>` it only moves your head pointer not your branch pointer , the moment you run this command then you are in a detached head state.
- `git checkout master` : it is use to recover the original state of branch and head pointer

- Examples:

```
git checkout d166439c1
Note: switching to 'd166439c1'.

You are in 'detached HEAD' state. You can look around, make experimental
changes and commit them, and you can discard any commits you make in this
state without impacting any branches by switching back to a branch.

If you want to create a new branch to retain commits you create, you may
do so (now or later) by using -c with the switch command. Example:

** git switch -c <new-branch-name> **

Or undo this operation with:

 `git switch -`

Turn off this advice by setting config variable advice.detachedHead to false

HEAD is now at d166439 added Advancedgitconcept.md
ritesh@atsp-4524:~/Desktop/riteshApp/MicroService-Project/GIT and GITHUB$ git log
commit d166439c1ab88f53f895f27f6d4d5ea71f2ab44d (HEAD)
Author: tesh-ri <ritesh.chauhan@tibilsolutions.com>
Date:   Sat Oct 18 12:50:46 2025 +0530

   added Advancedgitconcept.md

commit 2c237a772b379fd70d3830981cad0d84e04e92de
Author: tesh-ri <ritesh.chauhan@tibilsolutions.com>
Date:   Fri Oct 17 23:26:53 2025 +0530

   added more stuf about git

commit d40147ad413a7122bdfa4afd027d4c06ca9d79b8
Author: tesh-ri <ritesh.chauhan@tibilsolutions.com>
:
```

```
 git checkout master
Previous HEAD position was d166439 added Advancedgitconcept.md
Switched to branch 'master'
Your branch is up to date with 'origin1/master'.
ritesh@atsp-4524:~/Desktop/riteshApp/MicroService-Project/GIT and GITHUB$ git log
commit 542388812f69b69671e6ffc1a16c4f5ce56ee4c7 (HEAD -> master, origin1/master, origin1/HEAD, newFeatures)
Author: tesh-ri <ritesh.chauhan@tibilsolutions.com>
Date:   Sun Oct 19 00:15:59 2025 +0530

    added opensource.md files and some img related to that

commit d166439c1ab88f53f895f27f6d4d5ea71f2ab44d
Author: tesh-ri <ritesh.chauhan@tibilsolutions.com>
Date:   Sat Oct 18 12:50:46 2025 +0530

    added Advancedgitconcept.md

commit 2c237a772b379fd70d3830981cad0d84e04e92de
:
```

- `git reset` : It is use when you want to move both of the things `head and branch pointer`.

  - git reset has three mode:

    1.  `soft mode` :Move HEAD (and the current branch) to another commit,
        but keep the changes in the staging area

    2.  `mixed mode` - not recommend to exec this command but this is `default mode`.
    3.  `hard mode` - same for this not recommend until or unless you needed to exec.

  - what git reset --soft <commitId> will do ?
    - it actually move your head and master and start pointing to that commitid. and previous commit will remove by the GC.
  - How to move back to the original commit , before executing any commits again?
    - `git reset ORIG_HEAD` this command will move back to the original commit until you have not done any commits on new commitsId.
  - There is more easy way to do git reset --soft <id> instead of this you can exec this command `git reset --soft head~`
  - The tilde (~) means “go back n generations in a straight line of ancestry (first parents only)”.
  - In simple words, HEAD~1 means the parent commit of HEAD.
    - head~ means move to the 1step back parent
    - head~2 means second anchestor and so on
    ```
    A -- B -- C -- D (HEAD)
         ↑
       HEAD~2
    ```
  - If you have done one reset and then you commited then also you can recove to the orginal head , but if you did one reset and another one reset then after you did commit then there is no way to move back to the original parent pointer. But if you want to recover then before doing reset you can save the commit id in `tags` and using this tag you are able to come back to the original parent.

```
| Mode                  | Moves HEAD? | Affects Staging Area?  | Affects Working Dir?        | Typical Use                  |
| --------------------- | ----------- | ---------------------- | --------------------------- | ---------------------------- |
| `--soft`              | ✅ Yes       | ❌ No (keeps staged)    | ❌ No (keeps changes)        | Redo commits                 |
| `--mixed` *(default)* | ✅ Yes       | ✅ Yes (unstages files) | ❌ No (keeps file changes)   | Undo commit but keep changes |
| `--hard`              | ✅ Yes       | ✅ Yes                  | ✅ Yes (discards everything) | Wipe all changes completely  |

```

- If you want to learn more about reset you could definetly checkout this page : https://www.atlassian.com/git/tutorials/undoing-changes/git-reset
