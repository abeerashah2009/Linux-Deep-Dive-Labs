# Lab 09: Working with Links

## Objective

Learn how to create and compare hard links and symbolic links in Linux.

---

## Prerequisites

- Basic Linux command-line knowledge
- Linux terminal access
- Understanding of files and inodes

---

## Task 1: Create the Original File

```bash
echo "This is the original file." > original.txt
```

Verify:

```bash
cat original.txt
```

---

## Task 2: Create a Hard Link

```bash
ln original.txt hardlink.txt
```

Verify:

```bash
ls
```

---

## Task 3: Create a Symbolic Link

```bash
ln -s original.txt symlink.txt
```

Verify:

```bash
ls
```

---

## Task 4: Compare File Details

```bash
ls -l
```

Observation:

- Hard link shares the same data as the original file.
- Symbolic link points to the original file.

---

## Task 5: Compare Inode Numbers

```bash
ls -i
```

Observation:

- `original.txt` and `hardlink.txt` have the same inode.
- `symlink.txt` has a different inode.

---

## Commands Learned

- ln
- ln -s
- ls -l
- ls -i

---

## Key Concepts

- A hard link points directly to the same inode as the original file.
- A symbolic link stores the path to another file.
- Hard links cannot span different file systems.
- Symbolic links can point to files or directories on different file systems.

---

## Result

Successfully created and compared hard links and symbolic links using Linux commands.
