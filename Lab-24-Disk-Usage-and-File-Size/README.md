# Lab 05: Disk Usage and File Size

## Objectives

* Understand how to determine disk usage and file sizes in Linux.
* Learn how to identify large files and directories.
* Learn how to analyze and manage disk space.
* Gain practical experience using the `du` and `sort` commands.

## Prerequisites

* Basic knowledge of Linux command line interface (CLI).
* Access to a Linux-based system.
* Basic understanding of the Linux file system hierarchy.

---

## Task 1: Checking Directory Sizes

### Step 1.1: Check the Size of a Single Directory

The `du` command is used to check disk usage.

Command:

```bash
du -sh /path/to/directory
```

For example, to check the size of the home directory:

```bash
du -sh ~
```

### Explanation

* `du` = Disk Usage
* `-s` = Shows only the summary
* `-h` = Displays the size in human-readable format such as KB, MB, or GB

### Example Output

```text
4.2G    /home/username
```

This means the home directory is using approximately 4.2 GB of disk space.

---

### Step 1.2: Check the Size of Subdirectories

Command:

```bash
du -sh /path/to/directory/*
```

For example:

```bash
du -sh ~/*
```

This displays the size of files and directories inside the home directory.

---

## Task 2: Analyzing Disk Usage by Directory

### Step 2.1: Identify the Largest Directories

The `sort` command can be combined with `du` to display the largest directories first.

Command:

```bash
du -sh /path/to/directory/* | sort -hr
```

Example:

```bash
du -sh ~/* | sort -hr
```

### Explanation

* `du -sh` displays directory sizes in human-readable format.
* `sort` sorts the results.
* `-h` allows sorting of human-readable sizes.
* `-r` reverses the order.

The largest directories will appear at the top.

---

## Task 3: Advanced Disk Usage Management

### Step 3.1: Analyze Files and Directories Recursively

The `-a` option includes both files and directories.

Command:

```bash
du -ah /path/to/directory | sort -hr
```

For example:

```bash
du -ah ~ | sort -hr
```

This command provides a detailed list of files and directories sorted from largest to smallest.

---

## Practical Commands Used

### Check Home Directory Size

```bash
du -sh ~
```

### Check Size of Home Directory Contents

```bash
du -sh ~/*
```

### Find Largest Directories

```bash
du -sh ~/* | sort -hr
```

### Find Largest Files and Directories

```bash
du -ah ~ | sort -hr
```

### Check Disk Space of File Systems

```bash
df -h
```

The `df -h` command shows the available and used disk space of mounted file systems.

---

## Observations

During this lab, the `du` command was used to determine the amount of disk space consumed by directories and files.

The `sort -hr` command was used to arrange the results from largest to smallest, making it easier to identify directories and files consuming significant storage.

The `df -h` command was also used to check overall disk space and available storage.

---

## Use Cases

Disk usage monitoring is important for Linux system administrators because it helps to:

* Identify large files and directories.
* Prevent disk space from becoming full.
* Improve system reliability.
* Manage server storage efficiently.
* Reduce the risk of service interruptions caused by insufficient disk space.

---

## Conclusion

This lab provided practical experience with Linux disk usage and file size management.

The `du` command was used to analyze the storage consumed by files and directories, while `sort -hr` was used to identify the largest items.

These commands are useful for regular Linux system administration and storage management. Further tools such as `ncdu` can also be explored for an interactive view of disk usage.
