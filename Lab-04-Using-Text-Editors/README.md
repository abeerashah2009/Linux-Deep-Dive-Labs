# Lab 04: Using Text Editors

## Objective

Learn how to create, edit, save, and exit files using the **nano** and **vi** text editors in Linux.

---

## Prerequisites

- Basic Linux command-line knowledge
- Terminal access
- nano and vi editors installed

---

## Task 1: Using nano

### Open/Create a File

```bash
nano example.txt
```

### Add the Following Text

```text
Hello, World!
This is a simple file edited using nano.
```

### Save and Exit

- Ctrl + O → Save
- Enter → Confirm filename
- Ctrl + X → Exit

### Verify

```bash
cat example.txt
```

Output:

```text
Hello, World!
This is a simple file edited using nano.
```

---

## Task 2: Using vi

### Open File

```bash
vi example.txt
```

### Enter Insert Mode

Press:

```text
i
```

### Add the Following Text

```text
Editing with vi is a bit different.
Remember to switch modes.
```

### Save and Exit

1. Press **Esc**
2. Type:

```text
:wq
```

3. Press **Enter**

### Verify

```bash
cat example.txt
```

Output:

```text
Hello, World!
This is a simple file edited using nano.
Editing with vi is a bit different.
Remember to switch modes.
```

---

## Commands Learned

- nano
- vi
- cat

---

## Key Concepts

- nano is beginner-friendly and easy to use.
- vi is a powerful editor commonly used by Linux system administrators.
- Insert Mode in vi allows text editing.
- Esc returns to Normal Mode.
- :wq saves the file and exits.

---

## Result

Successfully learned how to create, edit, save, and view files using the **nano** and **vi** text editors.
