# Lab 33: Basic Grep Usage

## Objectives

The objectives of this lab are:

* Understand the basic usage of the `grep` command.
* Search for text inside files.
* Search recursively inside directories.
* Display lines before and after a matching line.
* Understand basic pattern matching in Linux.

---

## Prerequisites

* Basic Linux command-line knowledge.
* Access to a Linux system such as Ubuntu.
* Basic knowledge of files and directories.

---

# Introduction to grep

`grep` is a Linux command used to search for text and patterns inside files.

The name `grep` stands for **Global Regular Expression Print**.

Basic syntax:

```bash
grep "search_term" filename
```

For example:

```bash
grep "error" sample.txt
```

This command searches for the word `error` inside `sample.txt`.

---

# Lab Tasks

## Task 1: Search for a String in a Single File

### Objective

Learn how to search for a specific string inside a file.

First, create a sample file:

```bash
nano sample.txt
```

Add the following content:

```text
network error at node 3
operation successful at node 5
network connection established
unexpected error at node 7
```

Save the file and verify its contents:

```bash
cat sample.txt
```

Now search for the word `error`:

```bash
grep "error" sample.txt
```

### Expected Result

The command displays:

```text
network error at node 3
unexpected error at node 7
```

This shows that `grep` displays only the lines containing the search term.

### Display Line Numbers

The `-n` option can be used to display line numbers:

```bash
grep -n "error" sample.txt
```

---

# Task 2: Search Recursively in Directories

### Objective

Learn how to search for text inside a directory and its subdirectories.

Create a directory:

```bash
mkdir logs
```

Create a file inside it:

```bash
nano logs/system.log
```

Add:

```text
system started
network error detected
service running
authentication error
```

Save the file.

Now search recursively:

```bash
grep -r "error" .
```

### Explanation

The `-r` option means **recursive**.

It searches the current directory and all its subdirectories.

### Case-Insensitive Search

We can also use:

```bash
grep -ri "error" .
```

The `-i` option makes the search case-insensitive.

---

# Task 3: Use Context Lines

### Objective

Display lines before and after a matching line.

Use:

```bash
grep -A 2 -B 2 "error" sample.txt
```

### Explanation

* `-A 2` displays **2 lines after** the matching line.
* `-B 2` displays **2 lines before** the matching line.

This is useful when the surrounding lines provide additional information about an error.

### Example

```bash
grep -A 2 -B 2 "error" sample.txt
```

This displays the matching `error` line together with two lines before and two lines after it.

---

# Practical grep Examples

### Search for a word

```bash
grep "error" sample.txt
```

### Search with line numbers

```bash
grep -n "error" sample.txt
```

### Search recursively

```bash
grep -r "error" .
```

### Ignore uppercase/lowercase differences

```bash
grep -i "error" sample.txt
```

### Search recursively and ignore case

```bash
grep -ri "error" .
```

### Show lines before and after a match

```bash
grep -A 2 -B 2 "error" sample.txt
```

---

# Useful grep Options

| Option | Description                         |
| ------ | ----------------------------------- |
| `-r`   | Search recursively                  |
| `-i`   | Ignore case                         |
| `-n`   | Show line numbers                   |
| `-A`   | Show lines after a match            |
| `-B`   | Show lines before a match           |
| `-C`   | Show lines before and after a match |
| `-v`   | Show lines that do not match        |
| `-c`   | Count matching lines                |

---

# Real-World Linux and DevOps Use

`grep` is commonly used by Linux and DevOps engineers for troubleshooting and log analysis.

For example:

```bash
grep -i "error" application.log
```

can be used to find errors in an application log.

A recursive search can be performed with:

```bash
grep -ri "error" /var/log/
```

Context can be displayed using:

```bash
grep -A 2 -B 2 "error" application.log
```

These commands are useful for:

* Log analysis
* Troubleshooting
* Searching configuration files
* Finding errors
* Investigating application problems
* Linux administration
* DevOps automation

---

# Verification

The following commands were practiced during this lab:

```bash
grep "error" sample.txt
```

```bash
grep -n "error" sample.txt
```

```bash
grep -r "error" .
```

```bash
grep -ri "error" .
```

```bash
grep -A 2 -B 2 "error" sample.txt
```

The files and directories were also checked using:

```bash
find . -type f
```

---

# Key Takeaways

* `grep` is used to search for text and patterns.
* `grep "error" file` searches for a specific word.
* `-r` searches directories recursively.
* `-i` ignores uppercase and lowercase differences.
* `-n` displays line numbers.
* `-A` displays lines after a match.
* `-B` displays lines before a match.
* `grep` is very useful for Linux troubleshooting and log analysis.

---

# Conclusion

In this lab, I learned the basic usage of the Linux `grep` command.

I practiced searching for strings in a single file, searching recursively through directories, and displaying context around matching lines.

These skills are important for Linux administration and DevOps because `grep` can quickly help identify errors, search configuration files, and analyze system and application logs.

---

# Lab Status

**Lab:** 33 - Basic Grep Usage

**Status:** Completed

**Environment:** Linux / Ubuntu

**Tool Used:** `grep`

**Focus:** Text Searching, Pattern Matching, and Log Analysis

---

## Author

**Abeera Shah**

Linux / DevOps Learning Portfolio
