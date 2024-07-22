## Configure Git
Let Git know who you are. This is important for version control systems, as each *Git commit uses this information*

```shell
git config --global user.name "aaryan-11-x"
git config --global user.email "aaryangolatkar@gmail.com"
git config --global init.default branch main
```

## Initialize Git
Once you have navigated to the correct folder, you can initialize Git on that folder

```sh
git init
Initialized empty Git repository in /Users/user/myproject/.git/
```

## Adding New Files
Files in your Git repository folder can be in one of 2 states:
-  `Tracked` - files that Git knows about and are *added* to the repository. If you make any changes to this file, git will keep a *track* about it.
-  `Untracked` - files that are in your working directory, but *not added* to the repository. Git won't care about any changes you make on this file(s).

 When you first add files to an empty repository, they are all untracked. To get Git to track them, you need to `stage` them, or add them to the staging environment.
 
Check the **status** of the git
```sh
git status

# OR

git status -s

# Short status flags are:-
# ?? - Untracked files
# A - Files added to stage
# M - Modified files
# D - Deleted files
```

To **Track** a File
```sh
git add [filename]

# To track all files in the current directory
git add -A OR git add .

# Track file even if it's in .gitignore
git add [filename] -f
```

This also means adding file(s) to the `staging environment`. It's like a holding pen until you add that entry.

To **Untrack** a File
```sh
git rm --cached [filename]
```

To **Remove a File** from the computer using `git`
```sh
git rm [filename]
```

To **Unstage** a File
```sh
git restore --staged [filename]
```

To **Discard Changes** in the working directory
```sh
git restore [filename]
# Ex: To restore a deleted file
```

---
# Git Commit
Adding commits keep track of our progress and changes as we work. Git considers each `commit` change point or "save point". It is a point in the project you can go back to if you find a bug, or want to make a change.
When we `commit`, we should **always** include a **message**.

```shell
git commit -m "First release of Hello World!"
[master (root-commit) 221ec6e] First release of Hello World!
 3 files changed, 26 insertions(+)
 create mode 100644 README.md
 create mode 100644 bluestyle.css
 create mode 100644 index.html
```

The `commit` command performs a commit, and the `-m "message"` adds a message.
The Staging Environment has been committed to our repo, with the message:  "First release of Hello World!"

**View** the changes
```sh
git diff
```

###### Git Commit without Stage
Sometimes, when you make small changes, using the staging environment seems like a waste of time. It is possible to commit changes directly, skipping the staging environment. The `-a` option will automatically stage every changed, already tracked file.
```sh
git commit -a -m "Updated index.html with a new line"
```


Change Comment of **HEAD** Commit
```sh
git commit -m "Filname renamed of image.png" --amend
```


##### Git Commit Log
To view the history of commits for a repository, you can use the `log` command
```sh
git log
# OR
git log --oneline
```

View the changes in **detail**
```sh
git log -p
```


##### Reset to Previous Commit
`reset` is the command we use when we want to move the repository back to a previous `commit`, discarding any changes made after that `commit`.

Step 1: Find the previous `commit`

![Git Reset Step 1](https://www.w3schools.com/git/img_reset_part1.gif)

Step 2: Move the repository back to that step
![[img_reset_part2.gif]]


```sh
git reset [commithash]
# commithash being the first 7 characters of the commit hash we found in the log
```


##### Git Undo Reset
Even though the commits are no longer showing up in the `log`, it is not removed from Git.

If you know the commit hash you can `reset` to it
```shell
git reset [commithash not shown in the log]
```

---
# Branches
- **Create** a new `branch`
```sh
git branch hello-world-images
```

- To **Print** ALL `Branches`
```sh
git branch 
	hello-world-images 
	* master

# "*" beside master specifies that we are currently on that branch
```

- To **move** b/w branches
```sh
git checkout [branch name]
# OR
git switch [branch name]
```

- To **create & move** to the new branch in one-go
```sh
git checkout -b [branch name]
# OR
git switch -c [branch name]
```

- **Merge** branches
```sh
git merge [branch name]
```

- **Delete** a branch
```sh
git branch -d [branch name]
```


#### Merge Conflict
To solve merge conflict, open that file in notepad, check which version of file you want to keep, & delete the rest of it, and then commit!