# Lab 49: Using and Creating Man Pages

## Introduction
In this lab, I learned how to use Linux manual pages and how to create my own custom man page for a Bash script.

## Objectives
- Learn how to use existing man pages.
- Search for Linux commands and their options.
- Create a simple Bash script.
- Create a custom manual page.
- Install and view the custom man page.

## Task 1: Using Existing Man Pages

First, I updated the package list and installed the required tools.

Commands:
```bash
sudo apt update
sudo apt install -y man-db groff-base
sudo mandb
```

I searched for the manual page of `ls`:

```bash
man -f ls
```

I viewed the main sections:

```bash
MANPAGER=cat man ls | col -b | grep -E '^(NAME|SYNOPSIS|DESCRIPTION)'
```

I searched for the `-a` option:

```bash
MANPAGER=cat man ls | col -b | grep -A2 -e '-a, --all'
```

**Learning:** The `-a` option displays hidden files and directories.

## Task 2: Creating a Bash Script

I created a working directory:

```bash
mkdir -p ~/manlab
cd ~/manlab
```

I created a script named `test_script.sh`.

Script content:

```bash
#!/bin/bash
echo "This is a test script."
```

I installed the script:

```bash
sudo install -D -m755 test_script.sh /usr/local/bin/test_script
```

I executed the script:

```bash
test_script
```

Output:
```text
This is a test script.
```

## Creating a Custom Man Page

I created a file named `test_script.1`.

I used these manual-page sections:

- `.TH` - Manual page title and header.
- `.SH NAME` - Command name and purpose.
- `.SH SYNOPSIS` - Command usage.
- `.SH DESCRIPTION` - Command explanation.
- `.SH AUTHOR` - Author information.

I previewed the manual page:

```bash
man -l test_script.1 | col -b | head
```

## Task 3: Installing and Viewing the Man Page

I compressed the manual page:

```bash
gzip -k test_script.1
```

I installed it in the Linux manual directory:

```bash
sudo install -D -m644 test_script.1.gz /usr/local/share/man/man1/test_script.1.gz
```

I updated the manual database:

```bash
sudo mandb
```

I checked the compressed file:

```bash
gzip -t /usr/local/share/man/man1/test_script.1.gz
```

I viewed the installed manual:

```bash
MANPAGER=cat man test_script | col -b | head
```

I searched for the custom manual page:

```bash
man -k test_script
```

## Important Commands

| Command | Purpose |
|---|---|
| `man` | View manual pages |
| `man -f` | Find a manual page description |
| `man -k` | Search manual pages |
| `mandb` | Update manual database |
| `gzip` | Compress files |
| `install` | Install files |
| `col -b` | Remove formatting characters |

## Conclusion

I successfully used existing Linux manual pages, created and executed a Bash script, wrote a custom man page, compressed and installed it, and verified it using the Linux `man` command.
