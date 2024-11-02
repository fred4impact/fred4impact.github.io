# Introduction to Git

Welcome to this tutorial on **Git**! This guide will help you learn the basics of Git with examples.

Git is a powerful and popular version control system that helps you track changes to code or files over time, collaborate with others, and manage different versions of your project efficiently. Here's a guide to get you started with Git and the most commonly used commands.

## Git Commands

### Installing Git on macOS
To install Git on macOS, use Homebrew:
```bash
brew install git


### Configure Git

Set up your name and email, which Git will use for all commits:

```
git config --global user.name "Your Name"
git config --global user.email "you@example.com"
````

```
Initialize a Repository
Initialize a new Git repository in your project:

git init

git clone <repository-url>
git status

git add <filename>

git add .

git commit -m "Your commit message here"

git log
git push origin <branch-name>