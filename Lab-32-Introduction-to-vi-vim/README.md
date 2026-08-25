# Lab 32: Introduction to vi/vim

## Overview

This lab provides a practical introduction to **vi/vim**, one of the most important text editors available on Unix/Linux systems.

vi and Vim are lightweight, powerful, and commonly available on Linux servers. They are especially useful for **DevOps, Linux administration, system administration, cloud engineering, and cybersecurity** because they allow administrators and engineers to edit configuration files directly from a terminal without requiring a graphical interface.

In this lab, I practiced opening files, entering Insert mode, editing text, saving changes, navigating through files, searching for text, and performing search-and-replace operations.

---

## Objectives

The main objectives of this lab were:

* Understand the basic functionality of the vi/vim editor.
* Learn the difference between Normal mode and Insert mode.
* Create and open files using vi/vim.
* Practice entering and exiting Insert mode.
* Add and modify text inside a file.
* Save changes using vi/vim commands.
* Navigate through files using keyboard commands.
* Search for specific text.
* Perform search-and-replace operations.
* Understand why vi/vim is useful for Linux administration and DevOps work.

---

## Prerequisites

Before starting this lab, the following knowledge was helpful:

* Basic Linux command-line knowledge.
* Familiarity with terminal commands.
* Basic understanding of Linux files and directories.
* Basic knowledge of creating and editing files.
* Access to a Linux environment.

---

# Introduction to vi/vim

### What is vi?

**vi** stands for **Visual Editor**. It is a traditional Unix/Linux text editor designed to work directly from the command line.

### What is Vim?

**Vim** stands for **Vi IMproved**. It is an enhanced version of vi that provides additional features such as:

* Syntax highlighting
* Multiple windows
* Multiple buffers
* Advanced search and replacement
* Plugins and extensions
* Improved navigation
* Customization
* Programming support

Because Vim works inside the terminal, it is extremely useful when working with remote Linux servers through SSH.

---

# Why vi/vim is Important for DevOps

vi/vim is an important skill for Linux and DevOps engineers because many production servers are managed through command-line interfaces.

For example, a DevOps engineer may need to edit:

* `/etc/hosts`
* `/etc/fstab`
* `/etc/ssh/sshd_config`
* `/etc/nginx/nginx.conf`
* `/etc/systemd/system/*.service`
* Shell scripts
* YAML configuration files
* Application configuration files
* Docker and Kubernetes configuration files

When working on a remote cloud server through SSH, a graphical text editor may not be available. vi/vim provides a reliable way to edit these files directly from the terminal.

---

# Lab Tasks

## Task 1: Create a File

First, I navigated to the Lab 32 directory:

```bash
cd ~/Linux-Deep-Dive-Labs/Lab-32-Introduction-to-vi-vim
```

I created a new file using the `touch` command:

```bash
touch file.txt
```

I verified that the file was created:

```bash
ls -l file.txt
```

### Expected Result

The command should display information about `file.txt`.

---

# Task 2: Open the File with vi

I opened the file using:

```bash
vi file.txt
```

If the file does not already exist, vi can create it when the file is saved.

The following command can also be used when Vim is installed:

```bash
vim file.txt
```

---

# Task 3: Understanding vi/vim Modes

One of the most important concepts in vi/vim is that the editor works using different modes.

## Normal Mode

Normal mode is the default mode when vi/vim starts.

It is mainly used for:

* Navigation
* Deleting text
* Copying text
* Pasting text
* Searching
* Running commands

Press:

```text
Esc
```

to return to Normal mode.

---

## Insert Mode

Insert mode is used to type and modify text.

Common commands for entering Insert mode include:

```text
i
```

Insert before the current character.

```text
a
```

Append after the current character.

```text
o
```

Open a new line below the current line.

To leave Insert mode:

```text
Esc
```

---

## Command Mode

Commands beginning with `:` are entered after pressing `Esc`.

Examples:

```text
:w
```

Save the file.

```text
:q
```

Quit vi/vim.

```text
:wq
```

Save and quit.

```text
:q!
```

Quit without saving changes.

---

# Task 4: Enter Insert Mode

After opening the file, I pressed:

```text
i
```

This entered Insert mode.

I then added sample text such as:

```text
Linux is an open-source operating system.
vi is a powerful command-line text editor.
Vim is an improved version of vi.
Linux administration requires command-line skills.
DevOps engineers frequently work with terminal-based tools.
```

After entering the text, I pressed:

```text
Esc
```

to return to Normal mode.

---

# Task 5: Save the File

After returning to Normal mode, I used:

```text
:w
```

The `:w` command means **write**, which saves the current changes to the file.

To save and exit at the same time:

```text
:wq
```

---

# Task 6: Navigation Practice

In Normal mode, vi/vim provides keyboard-based navigation.

| Key | Action     |
| --- | ---------- |
| `h` | Move left  |
| `j` | Move down  |
| `k` | Move up    |
| `l` | Move right |

These keys are commonly used instead of the arrow keys.

### Additional Navigation Commands

```text
0
```

Move to the beginning of the current line.

```text
$
```

Move to the end of the current line.

```text
gg
```

Move to the beginning of the file.

```text
G
```

Move to the end of the file.

```text
Ctrl + f
```

Move forward one screen.

```text
Ctrl + b
```

Move backward one screen.

---

# Task 7: Searching for Text

I practiced searching for a specific word inside the file.

First, I returned to Normal mode:

```text
Esc
```

Then I entered:

```text
/example
```

and pressed:

```text
Enter
```

vi/vim searches forward through the file for the specified text.

### Search Navigation

After performing a search:

```text
n
```

moves to the next matching result.

```text
N
```

moves to the previous matching result.

---

# Task 8: Search and Replace

I practiced replacing text using the substitution command:

```text
:%s/old/new/g
```

### Explanation

The command consists of several parts:

```text
:
```

Enters command mode.

```text
%
```

Applies the command to the entire file.

```text
s
```

Means substitution.

```text
old
```

The text that should be searched for.

```text
new
```

The replacement text.

```text
g
```

Replaces all matching occurrences on each line.

### Example

To replace every occurrence of `Linux` with `Ubuntu`:

```text
:%s/Linux/Ubuntu/g
```

---

# Task 9: Useful vi/vim Commands

| Command         | Description               |
| --------------- | ------------------------- |
| `i`             | Enter Insert mode         |
| `a`             | Append text after cursor  |
| `o`             | Create a new line below   |
| `Esc`           | Return to Normal mode     |
| `:w`            | Save file                 |
| `:q`            | Quit                      |
| `:wq`           | Save and quit             |
| `:q!`           | Quit without saving       |
| `dd`            | Delete current line       |
| `yy`            | Copy current line         |
| `p`             | Paste copied/deleted text |
| `u`             | Undo last change          |
| `Ctrl+r`        | Redo change               |
| `/text`         | Search for text           |
| `n`             | Next search result        |
| `N`             | Previous search result    |
| `gg`            | Go to beginning of file   |
| `G`             | Go to end of file         |
| `0`             | Beginning of line         |
| `$`             | End of line               |
| `:%s/old/new/g` | Replace all occurrences   |

---

# Task 10: Verify the File from the Terminal

After saving and exiting vi/vim, I verified the file from the Linux shell.

```bash
cat file.txt
```

I also checked the file information:

```bash
ls -lh file.txt
```

This confirmed that the file was successfully created and saved.

---

# Practical DevOps Connection

vi/vim skills are useful in real-world DevOps environments.

For example, while connected to an AWS EC2 Linux server through SSH, an engineer may need to edit a configuration file:

```bash
ssh user@server
```

After connecting to the server, a configuration file can be opened with:

```bash
sudo vi /etc/hosts
```

or:

```bash
sudo vi /etc/ssh/sshd_config
```

This demonstrates why terminal-based editors remain important even when working with modern cloud and DevOps technologies.

---

# Troubleshooting

## Problem 1: I cannot type normally

The editor may be in Normal mode.

Press:

```text
i
```

to enter Insert mode.

---

## Problem 2: I cannot exit the editor

Press:

```text
Esc
```

Then use:

```text
:q
```

If changes prevent you from quitting and you do not want to save them:

```text
:q!
```

---

## Problem 3: I want to save and exit

Press:

```text
Esc
```

Then:

```text
:wq
```

---

## Problem 4: I accidentally changed something

Use:

```text
u
```

in Normal mode to undo the last change.

---

# Verification Checklist

The following activities were practiced during this lab:

* [x] Created a text file using `touch`.
* [x] Opened the file using `vi`.
* [x] Entered Insert mode.
* [x] Added text to the file.
* [x] Returned to Normal mode.
* [x] Saved changes using `:w`.
* [x] Exited vi using `:q`.
* [x] Practiced `h`, `j`, `k`, and `l` navigation.
* [x] Practiced searching using `/`.
* [x] Practiced search-result navigation.
* [x] Practiced search and replace using `:%s/old/new/g`.
* [x] Verified the saved file from the Linux terminal.

---

# Skills Demonstrated

This lab demonstrates practical knowledge of:

* Linux command-line operations
* vi/vim text editing
* Linux file management
* Terminal-based configuration management
* File navigation
* Search and replace
* Command-line productivity
* Basic Linux administration
* Remote server administration concepts

---

# Conclusion

This lab provided a practical introduction to **vi/vim**, including file creation, editing, navigation, searching, saving, and replacing text.

The most important concept learned in this lab is the use of **vi/vim modes**. Understanding the difference between Normal mode, Insert mode, and Command mode makes the editor much easier to use.

vi/vim is an essential Linux skill for anyone working in **DevOps, Cloud, System Administration, Cybersecurity, or Site Reliability Engineering**, especially when managing remote Linux servers through SSH.

---

## Key Takeaways

* vi is a traditional Unix/Linux text editor.
* Vim is an enhanced version of vi.
* Normal mode is used for navigation and commands.
* Insert mode is used to enter text.
* `Esc` returns to Normal mode.
* `:w` saves changes.
* `:q` exits the editor.
* `:wq` saves and exits.
* `/text` searches for text.
* `:%s/old/new/g` performs global search and replace.
* vi/vim is highly useful for remote Linux server administration.

---

## Lab Status

**Lab 32: Introduction to vi/vim**

**Status:** Completed

**Environment:** Linux

**Primary Tool:** vi/vim

**Focus:** Linux Text Editing and Command-Line Productivity

---

## Author

**Abeera Shah**

Linux / DevOps Learning Portfolio
