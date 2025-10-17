# Here we will dicuss about the GIT

## GIT init

```Bash
git init

# it initialized a empty git repository in that folder
```

### It also creates a `.git` folder that contains all the relevant logics to manage the version control of your projects.

```txt
ls -a
ls -l -for more info about the files that are present
```

### if you again executed the same commnand `git init` then it will reini the git repo

### How to delete the .git folder and why its needed?

- `Delete` : `sudo rm -rf .git`
- It is needed in some cases like , if you want to add new git repo with this folder then you have to delte this folder. Now you can init the new repo

## Deep Dive Into Git

- There are total three area exists in git
- `working area` , `staged area` , `repository area`
- Lets talk about each of the area one by one

### Working Area

- There can be a bunch of file that are not currently handled by `git`.
- It means that chanages done or to be done in those files are not managed by `git`. A file which is in working area is considered to be not in staging area.
  When we do `git status` and we see abunch of unstaged file that is not tracked by `git` {`untracked file`} then these are actually called to be in working directory.

### Staging Area

- What all files are going to be a part of next version that we will create.
- This staging area is the place where git knows what changes will be done from last version to the next version(track krta hai ki kya kya changes hogye hai w.r.t previous version);
- How to move files from working directory to staging area

```Bash
  git add file_name , ....more file name
  git add .

```

- `git add .` : This command is use to add all the working directory file into staging area(`track by git`).
- `git add file_name_1 , file_name_2,...` : This command is used to add specific file/files(for more than one file add using comma seperated).
- Using this command you will get the new version of your project.
- Now your files are in stagged area

### How to unstaged the files

- Using this command you are able to unstaged the files
- It moves from staging area to working area.

```
git rm --cached file name
```

### Repositiory Area

- This area actually contains the details of all your previous registered version and the files in this area , git already manages them and knows their version history.
- In git versions are actually manage using `commits`
- `commits`: Commit is a particular version of the project.It captures a snapshot of the project`s staged changes and creates a version of it.
- `git commit` : After this command we are able to registered files in git.It registers staging changes to commit.
  It will open on vim based text editor where you can able to run vim command.
  You can add the commits and save it and exit the editor.
- `git log` -List downs all the commits of the repository.If you want to exit out of git log prompt press `q`.
- `git restore <file>` -If you want to get the older version and wanted to delte the code that is not in staging area then exe this command (it remove all the files changes from the staging area to be commited).This can be helpful , if we did some dirty piece of code and now we do not want this code. Instead of deleting every change line by line , we can restore it or you can say restore last clean version of the file.

- `git add <files>` then this piece of code move into staged area (from working area) and now if you want to restore into previous version then `git restore <files>` cmd would not work bcoz now this piece of code(files) is/are in `staged area` , so you need to first remove from `staged area` and add into `working area` using cmd `git restore --staged <files>` and now these files are in `working area` at this moment you can restore the previous version using cmd `git restore <files>`.
- But once you have commited the changes then all the staged area code now a part of commit so if you will exe this cmd `git restore --staged <files>` it would not work bcoz everythings are in the commit place(new version).
- Diff between `git rm`and `git restore` : `git rm --cached <fles>` will work only when if all files are in same area(either in working or in staged) if it is not there then it will give warning and even though if you wanted to exectue this command then you have to add `-f` flag to do this forcefully.
- If you executed it successfully then you will get this things as an ouptut

```
On branch main
Untracked files:
  (use "git add <file>..." to include in what will be committed)
        git.md

nothing added to commit but untracked files present (use "git add" to track)

```

- Which means it removed all the things from staged area to `untracked area`.

- If you exe `git rm -cached <files>` without adding all the things in same area then you will get this type of error:

```
git rm --cached
git.md
error: the following file has staged content different from both the
file and the HEAD:
    git.md
(use -f to force removal)

```

- Then what are exact diff? : So if you want to play between stage area to working area(vice versa) then you can use `git restore` command and if you want to move it on untracked area then use `git rm` command.
- If you want to `commit` without using vim/nano editor then you can use this command: `git commit -m "commit-msg" `
- `git remote add origin <url>` This commands help us to link local repo to github repo(connection between two repo).
- `origin` is the name of remote repo , you can give any name (like newrepo , repo1 , etc).If you want to see the name of your connection exe this command `git remote`.
- `git remote` : List down all the remote connection names.
- `git push  -u origin <branch name>`: here orgin is the name of your remote connection if it is diff then you need to add that name here.Using this commnad you can able to push all the local code in github repo.
- `Master` is the default branch you can get after creating a remote repo on github.
- `git remote rm <name of remote>` : This command deltes a remote connection.
- `git remote rename <oname> <newname>` : This command rename the remote connection.
- NOTE : The name of remote connection is used to established communication between two repository.
- `git add -A` or `git add .` : This will add all files into staged area.
- `git pull <remote-name> <branch-name>` : To pull(download) all the latest files or code present in remote repo but not in local repo.

### Recommended Practice to do

      -make changes
      -git add <file>
      -git commit
      -git pull
      -git push

## Merge Conflicts

- What is merge conflict ?
- `ans` : A merge conflict happens when Git cannot automatically merge changes from two branches because the same part of a file was changed differently in each branch.

## Example Scenario

Suppose you have a repository with a file `app.js`:

```javascript
console.log("Hello World");
```

```
You create a branch feature and change it to:

console.log("Hello from Feature branch");


Meanwhile, on main, someone changes the same line to:

console.log("Hello from Main branch");


Now you try to merge feature into main:

git checkout main
git merge feature


Git sees that both branches changed the same line differently, so it can’t automatically decide which version to keep.

What Git Shows

Git will mark the conflict in the file like this:


<<<<<<< HEAD → Your current branch (main)

======= → Separator between versions

>>>>>>> feature → Incoming branch changes (feature)

How to Resolve a Merge Conflict

Open the conflicted file.

Decide which version to keep — or combine them. For example:

console.log("Hello from both branches!");


Remove the conflict markers (<<<<<<<, =======, >>>>>>>).

Stage the resolved file:

git add app.js


Complete the merge with a commit:

git commit -m "Resolved merge conflict in app.js"

How to Check for Conflicts

After a merge fails:

git status


It will show something like:

both modified: app.js


→ Indicates that you need to manually resolve this file.

Tips to Avoid Merge Conflicts

Pull changes frequently (git pull) before starting new work.

Work on separate files whenever possible.

Communicate with your team to avoid editing the same lines at the same time.
```
