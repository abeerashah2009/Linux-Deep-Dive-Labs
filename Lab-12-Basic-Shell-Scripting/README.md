# Lab 12: Basic Shell Scripting

## Objective

Learn how to create, make executable, and run a basic shell script in Linux.

---

## Prerequisites

- Basic Linux command knowledge
- Linux terminal access
- Nano text editor

---

## Task 1: Create a Shell Script

Open Nano:

```bash
nano hello_world.sh
```

Script content:

```bash
#!/bin/bash

echo "Hello World!"
```

---

## Task 2: Verify the Script

Command:

```bash
cat hello_world.sh
```

Output:

```bash
#!/bin/bash

echo "Hello World!"
```

---

## Task 3: Make the Script Executable

Command:

```bash
chmod +x hello_world.sh
```

Verify:

```bash
ls -l
```

---

## Task 4: Execute the Script

Command:

```bash
./hello_world.sh
```

Output:

```text
Hello World!
```

---

## Commands Learned

- nano
- cat
- chmod
- ls -l
- ./script_name

---

## Key Concepts

- A shell script is a text file containing Linux commands.
- `#!/bin/bash` tells Linux to use the Bash shell.
- `chmod +x` makes a script executable.
- `./hello_world.sh` runs the script from the current directory.

---

## Result

Successfully created, made executable, and executed a basic Bash shell script.
