## How Internally Git Works ?

- In git Data structures plays and important role like: Hashing , Graph , Tree;

- You can assume that git is like key:value stores;
- `key` : stores the hash value of data;
- `value` : stores the actual data;
- `git` uses a cryptographic hash fucntion internally (`SHA1`) for a give data ; it returns an hashed 40 digit hexadecimalm number.
- The hash value is always same for same data;

- Git actually compress the data in blob(binary large object )format and also stores some metadata about data;
- the structures of data is : blob , <size> , '\o'->delemeter (content seperator) , actual content(compressed format).
- All these things goes inside the `.git` folder.
- Today we will going to reveal about `.git` folder;

## How to get `.git` folder?

- `git init` : when you execute this command a new empty repo init into that folder.
- Now after this command you will one `.git` folder{ls -a};
- How to check what are the things are present in .git folder?
- Exec this command `tree .git`
- If cmd would not work then download/install tree (works in linux and mac);
- Output:

```
tree .git
.git
├── branches
├── COMMIT_EDITMSG
├── config
├── description
├── HEAD
├── hooks
│   ├── applypatch-msg.sample
│   ├── commit-msg.sample
│   ├── fsmonitor-watchman.sample
│   ├── post-update.sample
│   ├── pre-applypatch.sample
│   ├── pre-commit.sample
│   ├── pre-merge-commit.sample
│   ├── prepare-commit-msg.sample
│   ├── pre-push.sample
│   ├── pre-rebase.sample
│   ├── pre-receive.sample
│   └── update.sample
├── index
├── info
│   └── exclude
├── logs
│   ├── HEAD
│   └── refs
│       ├── heads
│       │   └── master
│       └── remotes
│           └── origin1
│               └── master
├── objects
│   ├── 3d
│   │   └── ca6b0ce1f30be4f34f6b4d004ad6a4a0fd38d9
│   ├── 96
│   │   └── 18d4a6dc2c92570253338de8016b09b0a61d27
│   ├── d4
│   │   └── 0147ad413a7122bdfa4afd027d4c06ca9d79b8
│   ├── info
│   └── pack
└── refs
    ├── heads
    │   └── master
    ├── remotes
    │   └── origin1
    │       └── master
    └── tags

19 directories, 26 files

```

- How git convert data into hashed value ? -`ans` : `echo "some data" | git hash-object --stdin`
- what it will do ?
- It will take "some data" as an output (| pipe) and send it to hash-object which will generate the hash value;

```
echo "hello world" | git hash-object --stdin
3b18e512dba79e4c8300dd08aeb37f8e728b8dad
```

- You will get one 40digit hashed value (it`s always same if you hashed the same data again and again);

- Now whenever you will do `git add <file>, ..` it will create a file into `objects` (internally it call hash(filedata) return 40 digit hash value and the first two character will be the name of folder and rest all are the name of file);
- You can see inside the folder tree (inabove txt section);
- And this file content the blob data , how to see the data?
- exec this command : ` cd objects` -> `cd <two char of hashed value>` -> `cat <filename>`
- You will get a blob that.

```
cat 0147ad413a7122bdfa4afd027d4c06ca9d79b8

x��A� =�51Ư�t���o�9�$n�
\�r�NS�Q'2��H.[�}t��
                    �
                     :���]�]
w��+<z��J�KX_Rc�
                ��RyS���x��fg=[�{̅�̨��mm2�3���/2�J

```

- But i want to see the exact content not blob data
- Git provides us a cmd through which you can able to see the exact data , how?
- `git cat-file -p <folder+filename>`
- `Output` :

```
git cat-file -p d40147ad413a7122bdfa4afd027d4c06ca9d79b8
tree 3dca6b0ce1f30be4f34f6b4d004ad6a4a0fd38d9
author tesh-ri <ritesh.chauhan@gmail.com> 1760713454 +0530
committer tesh-ri <ritesh.chauhan@gmail.com> 1760713454 +0530

added some imp concepts of git
```

- see here in above cmd we have one flag
  - `-p`: use to print the data
  - `-t` : use to find the type of data {eg; tree , blob , etc}
  - whenever you are geeting tree as a type which means this contains sub-directory and much more;
- technically we have alot of directory and subdirectory IG we are missing some details about this like how git manages these things folder , sub folder , etc;

- Internally git uses tree (directed) and which contains two pointer : one pointing to the (blobs data ) and other one pointing the (tree {sub folder data});
  - And also it has some metadata about
  - types of pointer(blob , tree , commit, etc);
  - directory name or filename;
  - mode(permision whether file is exec file or not , etc);
- to manage directory git uses tree which contains the details about sub-tree , metadata , blob , etc;
- there is one root folder presnts in objects folder , which contains blob , tree (sub -directories) , commit;

```
git cat-file -p 3dca6b0ce1f30be4f34f6b4d004ad6a4a0fd38d9
100644 blob 9618d4a6dc2c92570253338de8016b09b0a61d27    git.md

```

- Here we are not geeting any tree data bcoz we donot have any folder and subfolder/files;
- commit contains almost everythings:
  - every commits hash value are diff even though if you have commited same data same commit message , why?
  - bcoz in every commit git is also adding the timestamp so this will always different that`s why we always get different commit id(makes unique);
  - after the first commit , commit will always contains one more details about the parent directory(branch) and parent commit;
  - you can also able to see the old commits , git also stores the old commit data;
- Git also do compression of data of each commit
- Every commit in Git creates objects (blobs, trees, commits) that represent the state of your files and folders.

- Instead of storing the entire file every time it changes, Git uses delta compression to store differences between file versions efficiently.
- When you push your code to a remote, Git may print something like:

```
Counting objects: 100, done.
Delta compression using up to 8 threads
```

- `Delta` compression means Git is:

  - Looking at similar objects (usually files that changed) in your commits.

  - Storing only the differences (deltas) instead of duplicating the entire file.

  - This is done in multiple stages:

    - First delta: Stores the difference between the current file and the previous version.

    - Diff-2 delta: Sometimes Git computes secondary deltas, which are deltas of deltas, to reduce storage even further.

- lets suppose if you have two differnt files with same data then it will creates same hash value instead of creating two diff , why? beacuse the content is same thats why git make it only one hash value(same data same hash value)
- git has garbage collector which is used to collect the old commits objecta and club it and add into a pack folder.
  - Git has a garbage collector (GC) that:
  - Cleans up these old/unreachable objects.
  - Optimizes repository storage.
  - Git groups multiple objects into a packfile (in .git/objects/pack/) to save space.
  - how to exec gc : `git gc`
  - `output`
  ```
  git gc
  Enumerating objects: 3, done.
  Counting objects: 100% (3/3), done.
  Delta compression using up to 8 threads
  Compressing objects: 100% (2/2), done.
  Writing objects: 100% (3/3), done.
  Total 3 (delta 0), reused 0 (delta 0)
  ```
