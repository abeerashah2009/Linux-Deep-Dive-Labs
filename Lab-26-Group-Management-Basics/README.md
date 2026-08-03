# Lab 26: Group Management Basics

## Overview

This lab demonstrates the fundamentals of Linux group management. Linux groups are used to organize users and manage access permissions to system resources.

The lab covers creating groups, adding users to supplementary groups, verifying group membership, and removing users from groups.

---

## Objectives

- Understand the purpose of Linux groups.
- Create and manage groups using `groupadd`.
- Add users to supplementary groups using `usermod`.
- Verify group membership using `groups` and `/etc/group`.
- Remove users from groups using `gpasswd`.
- Practice basic Linux user and group administration.

---

## Environment

- Operating System: Ubuntu Linux
- Shell: Bash
- User: `ubuntu`
- Repository: Linux-Deep-Dive-Labs
- Lab Directory: `Lab-26-Group-Management-Basics`

---

# Task 1: Create a New Group

## 1.1 Understanding `groupadd`

The `groupadd` command is used to create a new Linux group.

### Command

```bash
sudo groupadd developers
# Lab Execution Evidence

## Group Creation

Command:

```bash
sudo groupadd developers
