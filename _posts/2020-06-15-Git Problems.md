---
title: "A new task jumps into you current working progess?"
date: 2020-06-15T15:34:30-04:00
read_time: true
categories:
  - handbook
tags:
  - git
---

You are working on some changes, a task with higher priority jumps in. If this is the problem you faced, then my solution might give you some hints.

## Problem Clarification

I am using `git` as version control and `GitHub` as a remote hosting service.

1. I am working on some changes but there is an urgent task. I need to do some work and commit it.
2. Get my previous work changes back.

💡GOAL! Successfully completing the new work and come back to my work progress....

### Step One

Save the current working progress: `git stash`.

> If your changes include some untrack files, running `git stash -u` instead.

### Step Two

It is a good practice to check the status with remote branch before you want to make some changes. Otherwise, it's a pain to solve the conflicts sometimes. `git pull`. Then, make some changes and commit back.

### Step Three

Revert back to the previous stage: `git stash pop`.
