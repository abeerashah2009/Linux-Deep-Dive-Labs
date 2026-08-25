# Lab 01: Navigating the Filesystem

## Overview

This lab introduces the fundamental Linux filesystem navigation commands used to identify the current working directory, inspect directory contents, and move between directories.

The commands practiced in this lab are:

* `pwd` — Print the current working directory
* `ls` — List directory contents
* `ls -l` — Display directory contents in long format
* `ls -la` — Display all files, including hidden files, in long format
* `cd` — Change the current directory
* `cd ..` — Move to the parent directory

These commands are essential Linux administration skills and form the foundation for working with files, directories, permissions, processes, and system configuration.

---

## Objectives

By completing this lab, the following objectives were achieved:

* Understand the basic structure of the Linux filesystem.
* Determine the current working directory using `pwd`.
* List files and directories using `ls`.
* Display detailed directory information using `ls -l`.
* Display hidden files using `ls -la`.
* Navigate using relative directory paths.
* Navigate using absolute directory paths.
* Move to the parent directory using `cd ..`.
* Verify filesystem navigation using multiple Linux commands.

---

## Prerequisites

* Basic knowledge of the Linux command-line interface.
* Access to a Linux/Unix-like operating system.
* Terminal or SSH access.
* Basic understanding of files and directories.
* Git installed and configured.

---

## Lab Environment

| Item                  | Value                              |
| --------------------- | ---------------------------------- |
| Operating Environment | Linux                              |
| User                  | `ubuntu`                           |
| Hostname              | `ip-172-31-10-103`                 |
| Repository            | `Linux-Deep-Dive-Labs`             |
| Lab Directory         | `Lab-01-Navigating-the-Filesystem` |
| Shell                 | Bash                               |

---

# Task 1: Identifying the Current Directory

## Task 1.1: Using `pwd`

### Concept

The `pwd` command stands for **Print Working Directory**.

It displays the absolute path of the directory in which the user is currently working.

### Command

```bash
pwd
```

### Lab Execution

```bash
echo "===== Task 1.1: Current Working Directory ====="
pwd
```

### Output

```text
===== Task 1.1: Current Working Directory =====
/home/ubuntu/Linux-Deep-Dive-Labs/Lab-01-Navigating-the-Filesystem
```

### Explanation

The output confirms that the current working directory is:

```text
/home/ubuntu/Linux-Deep-Dive-Labs/Lab-01-Navigating-the-Filesystem
```

This is an **absolute path** because it begins at the root directory `/`.

### Verification

```bash
whoami
hostname
pwd
```

Output:

```text
ubuntu
ip-172-31-10-103
/home/ubuntu/Linux-Deep-Dive-Labs/Lab-01-Navigating-the-Filesystem
```

This confirms:

* Current user: `ubuntu`
* Hostname: `ip-172-31-10-103`
* Current working directory: Lab 01 directory

---

# Task 2: Exploring Directories

## Task 2.1: Listing Files with `ls`

### Concept

The `ls` command lists files and directories contained within the current directory.

### Command

```bash
ls
```

### Initial Result

When `ls` was first executed, the Lab 01 directory was empty, so no directory contents were displayed.

To provide practical filesystem objects for the navigation exercises, the following demonstration items were created:

```bash
touch navigation-demo.txt
mkdir filesystem-demo
touch .hidden-demo
```

This created:

* `navigation-demo.txt` — demonstration file
* `filesystem-demo` — demonstration directory
* `.hidden-demo` — hidden demonstration file

### Listing the Directory

```bash
ls
```

Output:

```text
filesystem-demo  navigation-demo.txt
```

### Observation

The hidden file `.hidden-demo` does not appear when using the normal `ls` command.

This demonstrates that Linux normally hides files whose names begin with `.`.

---

## Detailed Listing with `ls -l`

The `-l` option displays directory contents in **long listing format**.

### Command

```bash
ls -l
```

### Output

```text
total 4
drwxrwxr-x 2 ubuntu ubuntu 4096 Aug 25 00:31 filesystem-demo
-rw-rw-r-- 1 ubuntu ubuntu    0 Aug 25 00:31 navigation-demo.txt
```

### Understanding the Output

For the directory:

```text
drwxrwxr-x 2 ubuntu ubuntu 4096 ... filesystem-demo
```

The first character `d` indicates that `filesystem-demo` is a directory.

For the file:

```text
-rw-rw-r-- 1 ubuntu ubuntu 0 ... navigation-demo.txt
```

The first character `-` indicates that `navigation-demo.txt` is a regular file.

The listing also displays ownership, permissions, size, modification time, and filename.

---

# Task 2.2: Listing with Options

## Using `ls -la`

### Concept

The `ls` command accepts options that modify its behavior.

Two important options are:

| Option | Purpose                                |
| ------ | -------------------------------------- |
| `-l`   | Long/detailed listing format           |
| `-a`   | Show all files, including hidden files |

Combining them gives:

```bash
ls -la
```

### Command

```bash
echo "===== Task 2.2: ls -la ====="
ls -la
```

### Output

```text
total 12
drwxrwxr-x  3 ubuntu ubuntu 4096 Aug 25 00:31 .
drwxrwxr-x 31 ubuntu ubuntu 4096 Aug 25 00:30 ..
-rw-rw-r--  1 ubuntu ubuntu    0 Aug 25 00:31 .hidden-demo
drwxrwxr-x  2 ubuntu ubuntu 4096 Aug 25 00:31 filesystem-demo
-rw-rw-r-- 1 ubuntu ubuntu    0 Aug 25 00:31 navigation-demo.txt
```

### Important Entries

The output contains:

```text
.
..
.hidden-demo
filesystem-demo
navigation-demo.txt
```

The special directory entries have the following meanings:

* `.` represents the current directory.
* `..` represents the parent directory.
* `.hidden-demo` is a hidden file.
* `filesystem-demo` is a directory.
* `navigation-demo.txt` is a regular file.

The `-a` option makes `.hidden-demo`, `.`, and `..` visible.

---

# Task 3: Changing Directories

## Task 3.1: Navigating with `cd`

### Concept

The `cd` command means **Change Directory**.

It is used to move from one directory to another.

The general syntax is:

```bash
cd [directory_path]
```

There are two important types of paths:

1. Relative paths
2. Absolute paths

---

## Relative Path Navigation

A relative path is interpreted from the current working directory.

The Lab contains a directory named:

```text
filesystem-demo
```

The following command was used:

```bash
cd filesystem-demo
pwd
```

Output:

```text
/home/ubuntu/Linux-Deep-Dive-Labs/Lab-01-Navigating-the-Filesystem/filesystem-demo
```

This confirms successful navigation into `filesystem-demo`.

### Returning to the Lab Directory

The parent directory was accessed using:

```bash
cd ..
pwd
```

Output:

```text
/home/ubuntu/Linux-Deep-Dive-Labs/Lab-01-Navigating-the-Filesystem
```

This confirms that `cd ..` moved one level upward.

---

# Absolute Path Navigation

An absolute path begins from the root `/` directory.

The following absolute path was tested:

```bash
cd /home/ubuntu/Linux-Deep-Dive-Labs/Lab-01-Navigating-the-Filesystem
pwd
```

Output:

```text
/home/ubuntu/Linux-Deep-Dive-Labs/Lab-01-Navigating-the-Filesystem
```

This confirms that Linux can navigate directly to a directory using its complete filesystem path.

---

# Task 3.2: Navigating to the Parent Directory

### Concept

The special path:

```text
..
```

represents the parent directory.

Therefore:

```bash
cd ..
```

moves the user one directory level upward.

### Command

```bash
cd ..
pwd
```

The parent directory is:

```text
/home/ubuntu/Linux-Deep-Dive-Labs
```

The user can then return to the Lab 01 directory using:

```bash
cd Lab-01-Navigating-the-Filesystem
```

when starting from the repository root.

---

## Navigation Error Encountered During the Lab

The following command was also tested:

```bash
cd Lab-01-Navigating-the-Filesystem
```

while already located inside:

```text
/home/ubuntu/Linux-Deep-Dive-Labs/Lab-01-Navigating-the-Filesystem
```

Bash returned:

```text
-bash: cd: Lab-01-Navigating-the-Filesystem: No such file or directory
```

### Why This Happened

The command attempted to find another directory named:

```text
Lab-01-Navigating-the-Filesystem
```

inside the current Lab 01 directory.

That directory does not exist there.

The correct approach depends on the current location:

From the repository root:

```bash
cd Lab-01-Navigating-the-Filesystem
```

From anywhere using an absolute path:

```bash
cd /home/ubuntu/Linux-Deep-Dive-Labs/Lab-01-Navigating-the-Filesystem
```

From the Lab 01 directory itself, no `cd` command is required.

This error helped demonstrate an important Linux navigation concept: **relative paths are interpreted from the current working directory**.

---

# Final Verification

The complete lab verification was performed using:

```bash
echo "========================================"
echo "LAB 01 - FINAL VERIFICATION"
echo "========================================"

echo
echo "Current User:"
whoami

echo
echo "Hostname:"
hostname

echo
echo "Current Directory:"
pwd

echo
echo "Directory Contents:"
ls -la

echo
echo "Filesystem Demo Directory:"
ls -ld filesystem-demo

echo
echo "Navigation Demo File:"
ls -l navigation-demo.txt

echo
echo "Hidden File:"
ls -la .hidden-demo

echo
echo "========================================"
echo "LAB 01 VERIFICATION COMPLETE"
echo "========================================"
```

## Verification Result

```text
========================================
LAB 01 - FINAL VERIFICATION
========================================

Current User:
ubuntu

Hostname:
ip-172-31-10-103

Current Directory:
/home/ubuntu/Linux-Deep-Dive-Labs/Lab-01-Navigating-the-Filesystem

Directory Contents:
total 12
drwxrwxr-x  3 ubuntu ubuntu 4096 Aug 25 00:31 .
drwxrwxr-x 31 ubuntu ubuntu 4096 Aug 25 00:30 ..
-rw-rw-r--  1 ubuntu ubuntu    0 Aug 25 00:31 .hidden-demo
drwxrwxr-x  2 ubuntu ubuntu 4096 Aug 25 00:31 filesystem-demo
-rw-rw-r-- 1 ubuntu ubuntu    0 Aug 25 00:31 navigation-demo.txt

Filesystem Demo Directory:
drwxrwxr-x 2 ubuntu ubuntu 4096 Aug 25 00:31 filesystem-demo

Navigation Demo File:
-rw-rw-r-- 1 ubuntu ubuntu 0 Aug 25 00:31 navigation-demo.txt

Hidden File:
-rw-rw-r-- 1 ubuntu ubuntu 0 Aug 25 00:31 .hidden-demo

========================================
LAB 01 VERIFICATION COMPLETE
========================================
```

---

# Command Reference

| Command        | Purpose                                 |
| -------------- | --------------------------------------- |
| `pwd`          | Display the current working directory   |
| `ls`           | List visible directory contents         |
| `ls -l`        | List contents in detailed/long format   |
| `ls -a`        | Show hidden files                       |
| `ls -la`       | Show all files in detailed format       |
| `cd directory` | Enter a directory using a relative path |
| `cd /path`     | Navigate using an absolute path         |
| `cd ..`        | Move to the parent directory            |
| `whoami`       | Display the current user                |
| `hostname`     | Display the system hostname             |

---

# Key Linux Filesystem Concepts

## Absolute Path

An absolute path starts from the filesystem root:

```text
/
```

Example:

```text
/home/ubuntu/Linux-Deep-Dive-Labs/Lab-01-Navigating-the-Filesystem
```

Absolute paths provide the complete location of an object.

## Relative Path

A relative path is interpreted from the current working directory.

Example:

```bash
cd filesystem-demo
```

## Current Directory

The current directory is represented by:

```text
.
```

## Parent Directory

The parent directory is represented by:

```text
..
```

## Hidden Files

Linux treats filenames beginning with `.` as hidden.

Example:

```text
.hidden-demo
```

They can be displayed using:

```bash
ls -a
```

or:

```bash
ls -la
```

---

# Learning Outcomes

After completing this lab, the following skills were demonstrated:

* Identifying the current filesystem location.
* Understanding absolute and relative paths.
* Listing files and directories.
* Identifying hidden files.
* Reading long-format directory listings.
* Understanding `.` and `..`.
* Moving between directories using `cd`.
* Navigating to directories using absolute paths.
* Returning to parent directories.
* Troubleshooting an incorrect relative path.
* Verifying filesystem navigation using multiple commands.

---

# Lab Completion Status

| Task                                  | Status      |
| ------------------------------------- | ----------- |
| Identify current directory with `pwd` | ✅ Completed |
| List directory contents with `ls`     | ✅ Completed |
| Detailed listing with `ls -l`         | ✅ Completed |
| Display hidden files with `ls -la`    | ✅ Completed |
| Navigate using relative path          | ✅ Completed |
| Navigate using absolute path          | ✅ Completed |
| Navigate to parent with `cd ..`       | ✅ Completed |
| Verify filesystem contents            | ✅ Completed |
| Final verification                    | ✅ Passed    |

**Lab 01 Status: COMPLETE ✅**

---

# Conclusion

This lab successfully established the fundamental Linux filesystem navigation skills required for subsequent Linux administration and DevOps labs.

The practical exercises demonstrated how to identify the current working directory, inspect directory contents, recognize hidden files, distinguish between absolute and relative paths, and move through the filesystem using `cd`.

These commands are foundational for Linux system administration, shell scripting, DevOps, automation, and server management.
