---
title: "Things I wish I knew about Git"
date: 2026-04-14
draft: false
tags: ["git", "tools"]
summary: "After years of using Git daily, here are the commands and concepts that actually changed how I work."
---

After years of using Git daily, I still learn something new about it. Here are the things that actually changed how I work.

## Interactive rebase is underrated

Most people know `git rebase` for keeping a branch up to date. But `git rebase -i HEAD~5` lets you rewrite your last 5 commits — squash them, reorder them, edit messages. It's the difference between a messy commit history and one that reads like a story.

## `git stash` has subcommands

`git stash` alone is fine. But `git stash list`, `git stash pop stash@{2}`, and `git stash branch new-branch stash@{0}` are where the real power is. You can manage multiple stashes and turn any stash into a branch.

## Bisect finds bugs fast

`git bisect start`, mark a known good commit, mark the bad one, and Git does a binary search through your history to find exactly which commit introduced the bug. I use this maybe once a month and it saves hours every time.

## The reflog is your safety net

`git reflog` shows every position HEAD has been — including after resets, rebases, and amends. If you think you've lost work, you almost certainly haven't. Find the commit hash in the reflog and `git checkout` to it.

These aren't obscure tricks. They're just things that take a while to stumble across. Hopefully now you don't have to wait.
