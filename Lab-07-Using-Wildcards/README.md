# Lab 07: Using Wildcards

## Objective

The objective of this lab is to understand how to use Linux wildcards for efficient file management. This lab demonstrates how to copy and remove multiple files using pattern matching with the `*` (asterisk) and `?` (question mark) wildcards.

---

## Prerequisites

- Basic knowledge of Linux commands
- Familiarity with:
  - `cp` (Copy)
  - `rm` (Remove)
  - `ls` (List files)
  - `cd` (Change directory)

---

## Key Concepts

### What are Wildcards?

Wildcards are special characters used to match multiple files or directories without specifying each filename individually.

### Types of Wildcards

| Wildcard | Description | Example |
|----------|-------------|---------|
| `*` | Matches zero or more characters | `*.txt` |
| `?` | Matches exactly one character | `file?.txt` |

---

# Task 1: Copy Multiple Files Using the `*` Wildcard

## Step 1: Create Sample Files

```bash
touch file1.txt file2.txt report1.txt report2.txt
```

Verify:

```bash
ls
```

Output:

```text
file1.txt
file2.txt
report1.txt
report2.txt
```

---

## Step 2: Create a Backup Directory

```bash
mkdir backup
```

Verify:

```bash
ls
```

Output:

```text
backup
file1.txt
file2.txt
report1.txt
report2.txt
```

---

## Step 3: Copy All Text Files

```bash
cp *.txt backup/
```

Explanation:

- `cp` copies files.
- `*` matches any number of characters.
- `*.txt` selects all text files.
- `backup/` is the destination directory.

---

## Step 4: Verify the Copied Files

```bash
cd backup
```

```bash
ls
```

Output:

```text
file1.txt
file2.txt
report1.txt
report2.txt
```

---

# Task 2: Remove Files Using the `?` Wildcard

Return to the parent directory:

```bash
cd ..
```

Delete files:

```bash
rm file?.txt
```

Explanation:

The `?` wildcard matches exactly one character.

Deleted:

- file1.txt
- file2.txt

Not Deleted:

- report1.txt
- report2.txt

---

## Verify

```bash
ls
```

Output:

```text
backup
report1.txt
report2.txt
```

---

# Commands Used

| Command | Description |
|----------|-------------|
| `touch` | Create empty files |
| `mkdir` | Create a directory |
| `cp` | Copy files |
| `rm` | Remove files |
| `ls` | List files |
| `cd` | Change directory |

---

# Wildcard Summary

| Wildcard | Meaning |
|----------|---------|
| `*` | Matches zero or more characters |
| `?` | Matches exactly one character |

---

# Key Learning Outcomes

- Learned how to use the `*` wildcard to copy multiple files.
- Learned how to use the `?` wildcard to remove selected files.
- Understood pattern matching in Linux.
- Practiced file management using Linux command-line tools.
- Improved efficiency by performing operations on multiple files at once.

---

# Conclusion

In this lab, I learned how to use Linux wildcards to perform file operations efficiently. The `*` wildcard allows copying multiple files that match a pattern, while the `?` wildcard helps select files with exactly one matching character. Wildcards simplify file management, reduce repetitive typing, and improve productivity when working with the Linux command line.
