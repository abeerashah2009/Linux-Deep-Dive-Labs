# Lab 50: Introduction to Shell Variables

## Introduction

In this lab, I learned about shell variables in Linux Bash. I practiced creating local and global (exported) variables and using special shell variables.

## Objectives

- Understand shell variables.
- Create and use local variables.
- Create and use exported variables.
- Understand special variables such as `$?`, `$$`, and `$#`.

## Prerequisites

- Ubuntu 24.04
- Bash 5.2.21
- Basic Linux command-line knowledge.

## Task 1: Working with Local Variables

### Step 1: Create a Local Variable

Command:

```bash
MYVAR="HelloWorld"
```

### Step 2: Display the Variable

Command:

```bash
echo "$MYVAR"
```

Output:

```text
HelloWorld
```

### Step 3: Test Variable in Child Shell

Command:

```bash
bash -c 'echo "[$MYVAR]"'
```

Output:

```text
[]
```

**Learning:** A local variable is available in the current shell but is not automatically passed to a child shell.

## Task 2: Working with Global Variables

### Step 1: Create an Exported Variable

Command:

```bash
export MYVAR2="GlobalHello"
```

### Step 2: Access the Variable in a Child Shell

Command:

```bash
bash -c 'echo "[$MYVAR2]"'
```

Output:

```text
[GlobalHello]
```

### Step 3: Compare Both Variables

Command:

```bash
bash -c 'echo "[$MYVAR]"; echo "[$MYVAR2]"'
```

Output:

```text
[]
[GlobalHello]
```

**Learning:** An exported variable is available to child processes.

## Task 3: Understanding Special Variables

### Step 1: Exit Status Variable ($?)

Commands:

```bash
true; echo $?
false; echo $?
```

Output:

```text
0
1
```

**Learning:**
- `0` means the command completed successfully.
- `1` indicates failure in this example.

### Step 2: Process ID Variable ($$)

Commands:

```bash
echo $$
bash -c 'echo $$'
```

Output:

```text
2991
6014
```

**Learning:** `$$` displays the process ID of the current shell. The child shell has a different process ID.

### Step 3: Argument Count Variable ($#)

Commands:

```bash
bash -c 'echo $#'
bash -c 'echo $#' _ a b c
```

Output:

```text
0
3
```

**Learning:** `$#` displays the number of arguments supplied to a script or function.

### Step 4: Create an Arguments Script

Command:

```bash
cat > args.sh <<'EOF'
#!/bin/bash
echo "Arguments: $#"
EOF
```

Give execution permission:

```bash
chmod +x args.sh
```

Execute the script:

```bash
./args.sh one two three four
```

Output:

```text
Arguments: 4
```

## Important Commands

| Command | Purpose |
|---|---|
| `MYVAR="HelloWorld"` | Create a local variable |
| `echo "$MYVAR"` | Display a variable |
| `export MYVAR2="GlobalHello"` | Export a variable |
| `bash -c` | Execute a command in a child shell |
| `$?` | Show the last command's exit status |
| `$$` | Show the current shell process ID |
| `$#` | Show the number of arguments |
| `chmod +x` | Give execution permission |

## Conclusion

In this lab, I successfully learned how to create and use local and exported shell variables. I also practiced special shell variables `$?`, `$$`, and `$#` and created a Bash script to count command-line arguments.
