# Lab 10: Understanding Shells

## Objective

Learn how to identify the default shell, list available shells, and switch between different shells in Linux.

---

## Prerequisites

- Basic Linux command-line knowledge
- Linux terminal access

---

## Task 1: Check the Default Shell

Command:

```bash
echo $SHELL
```

Example Output:

```text
/bin/bash
```

---

## Task 2: List Available Shells

Command:

```bash
cat /etc/shells
```

Example Output:

```text
/bin/sh
/bin/bash
/bin/dash
```

---

## Task 3: Switch to Another Shell

Switch to sh:

```bash
sh
```

Verify:

```bash
echo $0
```

Output:

```text
sh
```

Return to bash:

```bash
exit
```

Verify:

```bash
echo $0
```

Output:

```text
bash
```

---

## Commands Learned

- echo
- cat
- sh
- exit

---

## Key Concepts

- A shell is a command-line interpreter.
- `$SHELL` displays the default login shell.
- `/etc/shells` lists all installed shells.
- `sh` starts a new shell session.
- `exit` returns to the previous shell.

---

## Result

Successfully checked the default shell, listed available shells, switched to another shell, and returned to the original shell.
