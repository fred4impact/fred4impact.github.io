# Introduction Git
Welcome to this tutorial on **Git**! This guide will help you learn the basics of Git with examples.



*Git is a powerful and popular version control system that helps you track changes to code or files over time, collaborate with others, and manage different versions of your project efficiently. Here's a guide to get you started with Git and the most commonly used commands.*

## Git Commands

Here's are most use gti commands:
installing git on macOS

```brew install git


Configure git
```
git config --global user.name "Your Name"
git config --global user.email "you@example.com"

git init

git clone <repository-url>


git status

git add <filename>  # Add a specific file
git add .           # Add all files in the directory

git commit -m "Your commit message here"

git log

git push origin <branch-name>

git pull origin <branch-name>

#### Branching and Merging

```
git branch <new-branch-name>
git checkout <branch-name>
git checkout -b <new-branch-name>
git checkout <target-branch>
git merge <source-branch>


#### Undoing Changes

```
git reset <filename>
git checkout -- <filename>
git revert <commit-id>
git reset --hard <commit-id>

#### Command	Description

git init	Initialize a new Git repository
git clone <url>	Clone a remote repository
git add <file>	Stage changes for commit
git commit -m "<msg>"	Commit staged changes
git status	Check the status of the working directory
git log	View commit history
git push	Push changes to the remote repository
git pull	Pull changes from a remote repository
git branch <name>	Create a new branch
git checkout <name>	Switch to a branch
git merge <branch>	Merge a branch into the current branch
git reset	Unstage changes
git revert <commit-id>	Revert a specific commit
git reset --hard <commit-id>	Reset to a specific commit, discarding later 

