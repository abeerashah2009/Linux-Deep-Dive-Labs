# Lab 35: awk for Data Processing

## Objectives

The objectives of this lab are:

* Understand the basic syntax and functionality of `awk`.
* Learn how to print specific columns from a text file.
* Filter rows based on a condition.
* Understand how `awk` can be used for data processing in Linux.

---

## Prerequisites

* Basic Linux command-line knowledge.
* Basic understanding of files and directories.
* Access to a Unix/Linux terminal.

---

# Introduction to awk

`awk` is a powerful Linux command used for **pattern scanning and text processing**.

It is especially useful when working with structured text data such as:

* Log files
* Reports
* Tables
* Configuration files
* Command output

Basic syntax:

```bash
awk 'pattern { action }' filename
```

`awk` automatically separates input into fields.

For example:

```text
Name Age Gender
John 28 Male
```

The fields are:

```text
$1 = Name
$2 = Age
$3 = Gender
```

`$0` represents the complete line.

---

# Lab Tasks

## Task 1: Print Specific Columns from a File

### Objective

Learn how to use `awk` to extract specific columns from a text file.

First, create a sample data file:

```bash
nano data.txt
```

Add the following content:

```text
Name Age Gender
John 28 Male
Emma 22 Female
Mike 32 Male
Lucy 29 Female
```

Save the file and verify it:

```bash
cat data.txt
```

---

## Print the First and Third Columns

Use:

```bash
awk '{print $1, $3}' data.txt
```

### Explanation

In the command:

```bash
awk '{print $1, $3}' data.txt
```

* `$1` represents the first column.
* `$3` represents the third column.
* `print` displays the selected columns.
* `data.txt` is the input file.

### Expected Output

```text
Name Gender
John Male
Emma Female
Mike Male
Lucy Female
```

This demonstrates how `awk` can extract specific fields from structured text.

---

# Task 2: Filter Rows Based on a Condition

### Objective

Use `awk` to filter rows based on a specific condition.

In this task, we will display users whose age is greater than 25.

Run:

```bash
awk '$2 > 25 {print $0}' data.txt
```

### Explanation

* `$2` represents the Age column.
* `> 25` checks whether the age is greater than 25.
* `$0` represents the complete row.
* Only rows that satisfy the condition are displayed.

### Expected Output

```text
John 28 Male
Mike 32 Male
Lucy 29 Female
```

---

# Understanding awk Fields

For the following row:

```text
John 28 Male
```

`awk` treats the values as fields:

| Field | Value        |
| ----- | ------------ |
| `$1`  | John         |
| `$2`  | 28           |
| `$3`  | Male         |
| `$0`  | John 28 Male |

This field-based approach makes `awk` very useful for processing structured text.

---

# Useful awk Commands

### Print the first column

```bash
awk '{print $1}' data.txt
```

### Print the second column

```bash
awk '{print $2}' data.txt
```

### Print the first and third columns

```bash
awk '{print $1, $3}' data.txt
```

### Print the complete line

```bash
awk '{print $0}' data.txt
```

### Filter rows where age is greater than 25

```bash
awk '$2 > 25 {print $0}' data.txt
```

---

# Practical Linux and DevOps Use

`awk` is commonly used by Linux and DevOps engineers for processing command output, logs, and structured text files.

For example, it can be used to extract information from command output:

```bash
ps aux | awk '{print $1, $2, $11}'
```

This can display selected fields from running processes.

Another example is processing log files:

```bash
awk '{print $1, $2, $3}' application.log
```

This can extract selected fields from a log file.

`awk` is also useful when combined with other Linux commands such as:

```bash
grep
sed
sort
cut
```

This makes it an important tool for shell scripting and automation.

---

# Verification

The following commands were practiced during this lab:

```bash
cat data.txt
```

```bash
awk '{print $1, $3}' data.txt
```

```bash
awk '$2 > 25 {print $0}' data.txt
```

The expected results were verified against the sample data.

---

# Key Takeaways

* `awk` is used for text and data processing.
* `$1` represents the first field.
* `$2` represents the second field.
* `$3` represents the third field.
* `$0` represents the complete line.
* `print` displays selected data.
* Conditions can be used to filter rows.
* `awk` is useful for logs, reports, command output, and automation.

---

# Conclusion

In this lab, I learned the basic functionality of the Linux `awk` command.

I practiced extracting specific columns from a text file using `$1` and `$3`, and I filtered rows using a condition based on the Age column.

These skills provide a foundation for more advanced text processing and are useful in Linux administration, DevOps, log analysis, data processing, and shell scripting.

---

# Lab Status

**Lab:** 35 - awk for Data Processing

**Status:** Completed

**Environment:** Linux / Ubuntu

**Tool Used:** `awk`

**Focus:** Data Processing, Column Extraction, Filtering, and Text Analysis

---

## Author

**Abeera Shah**

Linux / DevOps Learning Portfolio
