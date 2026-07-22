# Lab 11: Environment Variables

## Objective

Learn how to view, create, and remove environment variables in Linux.

---

## Prerequisites

- Basic Linux command knowledge
- Linux terminal access

---

## Task 1: View Environment Variables

Command:

```bash
env
```

Displays all environment variables.

---

## Task 2: Create an Environment Variable

Command:

```bash
export MYVAR="Hello"
```

Verify:

```bash
echo $MYVAR
```

Output:

```
Hello
```

---

## Task 3: Create Another Variable

Command:

```bash
export MYNAME="Abeera Shah"
```

Verify:

```bash
echo $MYNAME
```

Output:

```
Abeera Shah
```

---

## Task 4: Open a Child Shell

Command:

```bash
sh
```

Verify:

```bash
echo $MYNAME
```

Exit:

```bash
exit
```

---

## Task 5: Remove a Variable

Command:

```bash
unset MYVAR
```

Verify:

```bash
echo $MYVAR
```

Output:

(blank)

---

## Commands Learned

- env
- export
- echo
- unset
- sh
- exit

---

## Key Concepts

- Environment variables store configuration values.
- `env` displays all environment variables.
- `export` creates an environment variable available to child processes.
- `unset` removes an environment variable.
- `echo $VARIABLE` displays the value of a variable.

---

## Result

Successfully viewed, created, verified, and removed environment variables in Linux.
