# Lab 08: Viewing File Contents

## Objective

Learn how to view and search file contents using the `head`, `tail`, `more`, and `grep` commands in Linux.

---

## Prerequisites

- Basic Linux command-line knowledge
- Linux terminal access

---

## Task 1: Create a Sample File

```bash
nano example.txt
```

Added multiple lines of text for practice.

---

## Task 2: View the Beginning of a File

Display the first 10 lines:

```bash
head example.txt
```

Display the first 5 lines:

```bash
head -n 5 example.txt
```

---

## Task 3: View the End of a File

Display the last 10 lines:

```bash
tail example.txt
```

Display the last 15 lines:

```bash
tail -n 15 example.txt
```

---

## Task 4: Browse a File

```bash
more example.txt
```

Navigation:

- Spacebar → Next page
- Enter → Next line
- q → Quit

---

## Task 5: Search Text

Search for "Linux":

```bash
grep "Linux" example.txt
```

Case-insensitive search:

```bash
grep -i "linux" example.txt
```

Recursive search:

```bash
grep -r "Linux" .
```

---

## Commands Learned

- cat
- head
- tail
- more
- grep

---

## Key Concepts

- `head` displays the beginning of a file.
- `tail` displays the end of a file.
- `more` allows interactive viewing of large files.
- `grep` searches for specific text within files.

---

## Result

Successfully learned how to view and search file contents using Linux commands.
