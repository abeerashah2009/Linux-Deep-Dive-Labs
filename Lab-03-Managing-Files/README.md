# Lab 03: Managing Files

## Objective

Learn how to create, delete, and view files using Linux commands.

---

## Task 1: Create an Empty File

Command:

```bash
touch myfile.txt
```

Verify:

```bash
ls
```

Output:

```text
myfile.txt
```

Description:

Creates an empty file.

---

## Task 2: Delete a File

Command:

```bash
rm myfile.txt
```

Verify:

```bash
ls
```

Output:

```text
(No output because the file was deleted.)
```

Description:

Removes the file permanently.

---

## Task 3: Create a File with Content

Command:

```bash
echo "Hello, this is a sample text file." > sample.txt
```

Description:

Creates a file and writes text into it.

---

## Task 4: View File Content

Command:

```bash
cat sample.txt
```

Output:

```text
Hello, this is a sample text file.
```

Description:

Displays the contents of the file.

---

## Task 5: View File with less

Command:

```bash
less sample.txt
```

Description:

Displays the file page by page.

Press **q** to exit.

---

## Commands Learned

- touch
- rm
- echo
- cat
- less
- ls

---

## Result

Successfully learned how to create, remove, and view files using Linux commands.
