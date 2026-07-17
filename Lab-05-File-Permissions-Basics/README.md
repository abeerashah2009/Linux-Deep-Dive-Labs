# Lab 05: File Permissions Basics

## Objective

Learn how to check and modify file permissions in Linux using the `ls -l` and `chmod` commands.

---

## Prerequisites

- Basic Linux command-line knowledge
- Linux terminal access
- Understanding of files and directories

---

## Task 1: Check File Permissions

### Create a File

```bash
touch example.txt
```

### Check Permissions

```bash
ls -l
```

Example Output:

```text
-rw-rw-r-- 1 ubuntu ubuntu 0 Jul 17 13:34 example.txt
```

### Permission Breakdown

| Symbol | Meaning |
|--------|---------|
| - | Regular file |
| rw- | Owner can Read and Write |
| rw- | Group can Read and Write |
| r-- | Others can Read only |

---

## Task 2: Add Write Permission to Group

Command:

```bash
chmod g+w example.txt
```

Verify:

```bash
ls -l
```

Output:

```text
-rw-rw-r-- 1 ubuntu ubuntu 0 Jul 17 13:34 example.txt
```

Description:

Adds write permission to the group. In this lab, the group already had write permission, so no visible change occurred.

---

## Task 3: Change Permissions Using Numeric Mode

Command:

```bash
chmod 764 example.txt
```

Verify:

```bash
ls -l
```

Output:

```text
-rwxrw-r-- 1 ubuntu ubuntu 0 Jul 17 13:34 example.txt
```

### Meaning of 764

| Number | Permission |
|---------|------------|
| 7 | Read + Write + Execute (rwx) |
| 6 | Read + Write (rw-) |
| 4 | Read Only (r--) |

---

## Commands Learned

- ls -l
- chmod g+w
- chmod 764

---

## Key Concepts

- Linux permissions are divided into Owner, Group, and Others.
- Symbolic mode (`g+w`) modifies permissions using letters.
- Numeric mode (`764`) modifies permissions using octal values.
- Proper permissions improve system security and file accessibility.

---

## Result

Successfully learned how to check and modify Linux file permissions using the `ls -l` and `chmod` commands.
