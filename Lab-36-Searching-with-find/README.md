# 🔎 Lab 36: Searching with `find`

## 📌 Lab Overview

This lab introduces the Linux `find` command, a powerful utility for searching and locating files and directories based on specific criteria such as **name, size, and other attributes**.

The lab also demonstrates how to use the `-exec` option to perform additional operations on files discovered during a search.

---

## 🎯 Objectives

By completing this lab, you will:

* Understand the purpose and syntax of the `find` command.
* Search for files based on their names and extensions.
* Find files larger than a specified size.
* Use `-exec` to execute commands on matching files.
* Understand the `{}` placeholder and `\;` terminator.
* Develop practical Linux file-management skills.

---

## 🛠️ Prerequisites

* Basic Linux command-line knowledge.
* Access to a Linux/Unix-based operating system.
* Access to a terminal or shell.
* Basic understanding of Linux directories and file paths.

---

## 📚 Introduction

The `find` command is commonly used by Linux administrators and DevOps engineers to locate files and directories efficiently.

### Basic Syntax

```bash
find [path] [options] [expression]
```

For example:

```bash
find ~ -name "*.txt"
```

This searches the user's home directory for files ending in `.txt`.

---

# 🧪 Lab Tasks

## Task 1: Find All `.txt` Files in the Home Directory

### Command

```bash
find ~ -name "*.txt"
```

### Explanation

| Component | Description                                  |
| --------- | -------------------------------------------- |
| `find`    | Starts the file search                       |
| `~`       | Represents the current user's home directory |
| `-name`   | Searches according to the filename           |
| `"*.txt"` | Matches files ending with `.txt`             |

### Expected Result

The command displays the paths of matching `.txt` files found under the home directory.

### Example

```text
/home/ubuntu/notes.txt
/home/ubuntu/Documents/report.txt
/home/ubuntu/labs/example.txt
```

---

## Task 2: Find Files Larger Than 1 MB

### Command

```bash
find ~ -size +1M
```

### Explanation

The `-size` option allows files to be searched according to their size.

* `+1M` → files larger than 1 MiB
* `1M` → approximately 1 MiB
* `-1M` → smaller than 1 MiB

### Expected Result

The command displays files in the home directory that exceed the specified size.

### More Practical Example

To search only regular files:

```bash
find ~ -type f -size +1M
```

Here, `-type f` ensures that only regular files are returned.

---

## Task 3: Use `-exec` to Count Lines

The `-exec` option allows another command to be executed for every file found.

### Command

```bash
find ~ -name "*.txt" -exec wc -l {} \;
```

### Explanation

| Component       | Purpose                                     |
| --------------- | ------------------------------------------- |
| `find ~`        | Searches the home directory                 |
| `-name "*.txt"` | Finds `.txt` files                          |
| `-exec`         | Executes another command                    |
| `wc -l`         | Counts lines                                |
| `{}`            | Represents the current file found by `find` |
| `\;`            | Marks the end of the `-exec` command        |

### Example Output

```text
12 /home/ubuntu/notes.txt
25 /home/ubuntu/report.txt
8 /home/ubuntu/example.txt
```

The number before each filename represents the number of lines in that file.

---

# 🔐 Important `find` Concepts

## 1. Search by Name

```bash
find ~ -name "*.txt"
```

Searches for files with a specific name or extension.

---

## 2. Search by File Type

```bash
find ~ -type f
```

Finds regular files.

```bash
find ~ -type d
```

Finds directories.

---

## 3. Search by Size

```bash
find ~ -size +1M
```

Finds files larger than 1 MiB.

---

## 4. Execute Commands on Results

```bash
find ~ -name "*.txt" -exec wc -l {} \;
```

Runs `wc -l` against every matching file.

---

# 🧠 Command Summary

| Command                                  | Purpose                      |
| ---------------------------------------- | ---------------------------- |
| `find ~ -name "*.txt"`                   | Find `.txt` files            |
| `find ~ -size +1M`                       | Find files larger than 1 MiB |
| `find ~ -type f`                         | Find regular files           |
| `find ~ -type d`                         | Find directories             |
| `find ~ -name "*.txt" -exec wc -l {} \;` | Count lines in `.txt` files  |

---

# 💡 Additional Practice

Try experimenting with the following commands:

### Find `.log` Files

```bash
find ~ -name "*.log"
```

### Find Empty Files

```bash
find ~ -type f -empty
```

### Find Files Modified Within the Last Day

```bash
find ~ -type f -mtime -1
```

### Find Files Larger Than 10 MB

```bash
find ~ -type f -size +10M
```

### Display File Details

```bash
find ~ -type f -exec ls -lh {} \;
```

---

# ⚠️ Common Mistakes

### Forgetting Quotes Around Wildcards

Use:

```bash
find ~ -name "*.txt"
```

instead of:

```bash
find ~ -name *.txt
```

Quoting prevents the shell from expanding the wildcard before `find` receives it.

### Forgetting the `-exec` Terminator

Correct:

```bash
-exec wc -l {} \;
```

The `\;` tells `find` where the `-exec` expression ends.

---

# ✅ Lab Verification

Verify that the commands work correctly:

```bash
find ~ -name "*.txt"
```

```bash
find ~ -type f -size +1M
```

```bash
find ~ -name "*.txt" -exec wc -l {} \;
```

If the commands return the expected files and line counts, the lab tasks have been successfully completed.

---

# 🎓 Learning Outcomes

After completing this lab, you should be able to:

* Use `find` to search Linux files and directories.
* Filter search results using filenames and sizes.
* Distinguish between files and directories using `-type`.
* Use wildcards with filename searches.
* Execute commands against search results using `-exec`.
* Apply `find` in practical Linux administration and DevOps tasks.

---

## 🏁 Conclusion

The `find` command is an essential Linux administration tool for locating and managing files efficiently. In this lab, you practiced searching by **filename, file size, and file type**, and learned how to process matching files using `-exec`.

These techniques are useful in **Linux administration, system maintenance, automation, troubleshooting, and DevOps workflows**.

---

## 📌 Key Takeaway

> **`find` helps you locate files; `-exec` helps you perform actions on the files you locate.**

**Lab 36 — Searching with `find` ✅**
