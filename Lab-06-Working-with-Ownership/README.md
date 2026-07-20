# Lab 06: Working with Ownership

## Objective

Learn how to check and modify file ownership and group ownership in Linux using `ls -l`, `chown`, and `chgrp`.

---

## Prerequisites

- Basic Linux command-line knowledge
- Linux terminal access
- Understanding of file permissions

---

## Task 1: Check File Ownership

### Create a File

```bash
touch example.txt
```

### Check Ownership

```bash
ls -l
```

Example Output:

```text
-rw-rw-r-- 1 ubuntu ubuntu 0 Jul 17 15:30 example.txt
```

The first `ubuntu` is the owner and the second `ubuntu` is the group.

---

## Task 2: Change File Owner

### Create a New User

```bash
sudo useradd newuser
```

### Change Owner

```bash
sudo chown newuser example.txt
```

Verify:

```bash
ls -l
```

Example Output:

```text
-rw-rw-r-- 1 newuser ubuntu 0 Jul 17 15:30 example.txt
```

---

## Task 3: Change Group Ownership

### Create a New Group

```bash
sudo groupadd newgroup
```

### Change Group

```bash
sudo chgrp newgroup example.txt
```

Verify:

```bash
ls -l
```

Example Output:

```text
-rw-rw-r-- 1 newuser newgroup 0 Jul 17 15:30 example.txt
```

---

## Commands Learned

- ls -l
- useradd
- groupadd
- chown
- chgrp

---

## Key Concepts

- Every file has an owner and a group.
- `chown` changes the file owner.
- `chgrp` changes the file group.
- Proper ownership helps secure Linux systems.

---

## Result

Successfully learned how to check and change file ownership and group ownership using `chown` and `chgrp`.
