# Lab 34: sed for Text Manipulation

## Objectives

The objectives of this lab are:

* Understand the basic usage of the `sed` command.
* Replace text in a file using `sed`.
* Remove lines that match a specific pattern.
* Understand how `sed` can automate text editing tasks.

---

## Prerequisites

* Basic Linux command-line knowledge.
* Familiarity with text editors such as `nano`, `vi`, or `vim`.
* Access to a Unix/Linux system.
* Basic understanding of files and text manipulation.

---

# Introduction to sed

`sed` stands for **Stream Editor**.

It is a Linux command-line tool used to filter and transform text.

`sed` can be used for many text-processing tasks, including:

* Replacing text.
* Removing lines.
* Searching for patterns.
* Editing files automatically.
* Processing text from files or command pipelines.

Basic syntax:

```bash
sed 'command' filename
```

---

# Lab Tasks

## Task 1: Replace a Word in a File

### Objective

Learn how to replace a word using `sed`.

First, create a sample file:

```bash
echo -e "Hello World\nHello Universe\nGoodbye World" > example.txt
```

Verify the file:

```bash
cat example.txt
```

Expected output:

```text
Hello World
Hello Universe
Goodbye World
```

---

## Replace Text Using sed

Replace `World` with `Earth`:

```bash
sed 's/World/Earth/g' example.txt
```

Expected output:

```text
Hello Earth
Hello Universe
Goodbye Earth
```

### Explanation

The command:

```bash
sed 's/World/Earth/g' example.txt
```

contains:

* `s` = substitute.
* `World` = old text.
* `Earth` = new text.
* `g` = replace all matching occurrences on each line.

Important: without `-i`, the original file is **not changed**. The modified text is only displayed on the terminal.

---

## Save the Changes to the File

To modify the file directly, use:

```bash
sed -i 's/World/Earth/g' example.txt
```

Verify the changes:

```bash
cat example.txt
```

Expected output:

```text
Hello Earth
Hello Universe
Goodbye Earth
```

---

# Task 2: Remove Lines Matching a Pattern

### Objective

Learn how to remove complete lines containing a specific pattern.

In this example, we want to remove the line containing `Universe`.

First, view the file:

```bash
cat example.txt
```

Use:

```bash
sed '/Universe/d' example.txt
```

Expected output:

```text
Hello Earth
Goodbye Earth
```

### Explanation

The command:

```bash
sed '/Universe/d' example.txt
```

contains:

* `/Universe/` = search for the word `Universe`.
* `d` = delete the matching line.

The original file is not changed because `-i` was not used.

---

## Save the Deletion

To remove the matching line directly from the file:

```bash
sed -i '/Universe/d' example.txt
```

Verify the file:

```bash
cat example.txt
```

Expected output:

```text
Hello Earth
Goodbye Earth
```

---

# Useful sed Commands

### Replace text

```bash
sed 's/old/new/g' file.txt
```

### Replace text directly in the file

```bash
sed -i 's/old/new/g' file.txt
```

### Delete lines containing a pattern

```bash
sed '/pattern/d' file.txt
```

### Delete matching lines directly

```bash
sed -i '/pattern/d' file.txt
```

### Display specific lines

```bash
sed -n '1,2p' file.txt
```

This displays lines 1 through 2.

---

# Understanding `sed` Options

| Command/Option | Description                   |
| -------------- | ----------------------------- |
| `s`            | Substitute text               |
| `g`            | Replace all matches on a line |
| `d`            | Delete matching lines         |
| `-i`           | Modify the file directly      |
| `-n`           | Suppress normal output        |
| `p`            | Print selected lines          |

---

# Practical Linux and DevOps Use

`sed` is useful in real-world Linux and DevOps environments because it can automate repetitive text editing tasks.

For example, a DevOps engineer can use `sed` to:

* Modify configuration files.
* Update application settings.
* Remove unwanted configuration lines.
* Change environment values.
* Automate configuration changes in shell scripts.
* Process log and text files.

For example:

```bash
sed -i 's/PORT=8080/PORT=9090/g' application.conf
```

This changes the application port from `8080` to `9090`.

Another example:

```bash
sed -i '/debug=true/d' application.conf
```

This removes a configuration line containing `debug=true`.

---

# Verification

The following commands were practiced during this lab:

```bash
echo -e "Hello World\nHello Universe\nGoodbye World" > example.txt
```

```bash
cat example.txt
```

```bash
sed 's/World/Earth/g' example.txt
```

```bash
sed -i 's/World/Earth/g' example.txt
```

```bash
sed '/Universe/d' example.txt
```

```bash
sed -i '/Universe/d' example.txt
```

Finally, the file was verified using:

```bash
cat example.txt
```

Expected final output:

```text
Hello Earth
Goodbye Earth
```

---

# Important Difference: `sed` vs `sed -i`

Without `-i`:

```bash
sed 's/World/Earth/g' example.txt
```

The modified text is displayed, but the original file remains unchanged.

With `-i`:

```bash
sed -i 's/World/Earth/g' example.txt
```

The file itself is modified.

This difference is important when working with configuration files because an incorrect `sed -i` command can directly change a file.

---

# Key Takeaways

* `sed` is a powerful Linux stream editor.
* `s` is used for substitution.
* `g` replaces all matching occurrences on a line.
* `d` deletes matching lines.
* `-i` modifies the file directly.
* `sed` can automate repetitive text-editing tasks.
* `sed` is useful for Linux administration, DevOps, scripting, and configuration management.

---

# Conclusion

In this lab, I learned the basic usage of the Linux `sed` command for text manipulation.

I practiced replacing words inside a file using substitution and removing lines that matched a specific pattern.

I also learned the difference between displaying modified output and changing the original file using the `-i` option.

These skills are useful for Linux administration and DevOps because `sed` can automate configuration changes and other repetitive text-processing tasks.

---

# Lab Status

**Lab:** 34 - sed for Text Manipulation

**Status:** Completed

**Environment:** Linux / Ubuntu

**Tool Used:** `sed`

**Focus:** Text Manipulation, Substitution, Deletion, and Automation

---

## Author

**Abeera Shah**

Linux / DevOps Learning Portfolio
