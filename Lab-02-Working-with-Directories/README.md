# Lab 02: Working with Directories

## Objective
Learn how to create, remove, rename, and move directories in Linux.

---

## Task 1: Create a Directory

Command:

```bash
mkdir my_new_directory
```

Output:

```text
my_new_directory
```

Description:
Creates a new directory named `my_new_directory`.

---

## Task 2: Create a Subdirectory

Command:

```bash
mkdir my_new_directory/sub_directory
```

Verify:

```bash
ls my_new_directory
```

Output:

```text
sub_directory
```

Description:
Creates a subdirectory inside `my_new_directory`.

---

## Task 3: Remove Empty Directories

Commands:

```bash
rmdir my_new_directory/sub_directory
rmdir my_new_directory
```

Description:
Removes the empty subdirectory first, then removes the main directory.

---

## Task 4: Rename a Directory

Commands:

```bash
mkdir old_directory
mv old_directory new_directory
```

Output:

```text
new_directory
```

Description:
Renames `old_directory` to `new_directory`.

---

## Task 5: Move a Directory

Commands:

```bash
mkdir parent_directory
mv new_directory parent_directory/
```

Verify:

```bash
ls parent_directory
```

Output:

```text
new_directory
```

Description:
Moves `new_directory` into `parent_directory`.

---

## Commands Learned

- pwd
- ls
- mkdir
- rmdir
- mv

---

## Result

Successfully learned how to create, remove, rename, and move directories using Linux commands.
