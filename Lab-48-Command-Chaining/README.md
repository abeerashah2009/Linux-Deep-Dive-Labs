# Lab 48: Command Chaining

## Introduction

Command chaining is a fundamental Linux shell feature that allows multiple commands to be executed from a single command line. Shell operators can control whether subsequent commands execute based on the success or failure of previous commands.

In this lab, the three commonly used command chaining operators were explored:

* `&&` — Execute the next command only if the previous command succeeds.
* `||` — Execute the next command only if the previous command fails.
* `;` — Execute commands sequentially regardless of the previous command's success or failure.

The behavior of these operators was tested using successful and failing commands, followed by a practical backup scenario.

---

## Objectives

By completing this lab, the following objectives were achieved:

* Understand the concept of command chaining.
* Learn the purpose and behavior of `&&`.
* Learn the purpose and behavior of `||`.
* Learn the purpose and behavior of `;`.
* Observe command execution based on exit status.
* Compare the behavior of different chaining operators.
* Apply command chaining to a practical backup scenario.
* Understand the importance of operator evaluation when combining `&&` and `||`.

---

## Prerequisites

* Basic Linux command-line knowledge.
* Access to a Linux or Unix-like operating system.
* Familiarity with basic shell commands.
* Understanding of command exit status.

---

# Task 1: Using the `&&` Operator

## Objective

Understand how `&&` conditionally executes commands based on the success of the previous command.

The `&&` operator executes the command on its right only when the command on its left succeeds.

### Step 1: Two Successful Commands

The following command was executed:

```bash
mkdir new_directory && cd new_directory
```

The `mkdir` command successfully created the directory, so the `cd` command was executed.

The working directory was verified using:

```bash
pwd
```

Result:

```text
/home/ubuntu/Linux-Deep-Dive-Labs/Lab-48-Command-Chaining/new_directory
```

### Step 2: Successful Command Followed by a Failing Command

The following command was tested:

```bash
echo "FIRST COMMAND SUCCESS" && cd non_existent_directory && echo "THIS WILL NOT PRINT"
```

Observed behavior:

```text
FIRST COMMAND SUCCESS
bash: cd: non_existent_directory: No such file or directory
```

The final command:

```bash
echo "THIS WILL NOT PRINT"
```

was not executed because the `cd` command failed.

### Result

The `&&` operator was successfully demonstrated as a **success-dependent execution operator**.

### Evidence

* `evidence/task1-and-success.txt`
* `evidence/task1-and-failure.txt`

---

# Task 2: Using the `||` Operator

## Objective

Understand how `||` conditionally executes a command when the previous command fails.

The `||` operator executes the command on its right only when the command on its left fails.

### Step 1: Failed Command with Fallback

The following command was executed:

```bash
cd non_existent_directory || echo "Directory does not exist"
```

Observed behavior:

```text
bash: cd: non_existent_directory: No such file or directory
Directory does not exist
```

Because `cd` failed, the `echo` command was executed.

### Step 2: Successful Command with `||`

The following command was also tested:

```bash
echo "Successful command" || echo "This will NOT print"
```

Observed behavior:

```text
Successful command
```

The second `echo` command was skipped because the first command succeeded.

### Result

The `||` operator was successfully demonstrated as a **failure-dependent fallback operator**.

### Evidence

* `evidence/task2-or.txt`

---

# Task 3: Using the `;` Operator

## Objective

Understand sequential command execution using the semicolon operator.

The `;` operator executes commands sequentially regardless of whether the previous command succeeds or fails.

### Command Tested

```bash
echo "This will always print"; cd non_existent_directory; echo "This prints anyway"
```

Observed behavior:

```text
This will always print
bash: cd: non_existent_directory: No such file or directory
This prints anyway
```

The `cd` command failed, but the final `echo` command still executed.

### Result

The `;` operator was successfully demonstrated as a **sequential execution operator that does not depend on the previous command's success**.

### Evidence

* `evidence/task3-semicolon.txt`

---

# Task 4: Operator Comparison

The three command chaining operators were compared using the `true` and `false` commands.

`true` returns a successful exit status, while `false` returns a failure status.

## `&&` Tests

Successful command:

```bash
true && echo "SECOND COMMAND RUN"
```

Result:

```text
SECOND COMMAND RUN
```

Failed command:

```bash
false && echo "SECOND COMMAND WILL NOT RUN"
```

Result:

```text
The second command was skipped.
```

---

## `||` Tests

Failed command:

```bash
false || echo "SECOND COMMAND RUN BECAUSE FIRST FAILED"
```

Result:

```text
SECOND COMMAND RUN BECAUSE FIRST FAILED
```

Successful command:

```bash
true || echo "SECOND COMMAND WILL NOT RUN"
```

Result:

```text
The second command was skipped.
```

---

## `;` Test

```bash
false; echo "SECOND COMMAND RUNS ANYWAY"
```

Result:

```text
SECOND COMMAND RUNS ANYWAY
```

---

## Comparison Table

| Operator | Condition                 | Behavior                        |                        |                      |
| -------- | ------------------------- | ------------------------------- | ---------------------- | -------------------- |
| `&&`     | Previous command succeeds | Execute next command            |                        |                      |
| `        |                           | `                               | Previous command fails | Execute next command |
| `;`      | No condition              | Execute next command regardless |                        |                      |

### Evidence

* `evidence/operator-comparison.txt`
* `evidence/operator-summary.txt`

---

# Task 5: Backup Case Study

## Objective

Apply command chaining to a practical backup scenario.

A test file was created:

```bash
echo "Important backup data" > important_file.txt
```

The file was verified with:

```bash
cat important_file.txt
```

Result:

```text
Important backup data
```

---

## Supplied Case-Study Command

The lab provided the following command:

```bash
mkdir backup || echo "Backup directory already exists" && cp important_file.txt backup/
```

### Successful Execution

When the `backup` directory did not exist, `mkdir` successfully created it and the file was copied.

The backup was verified using:

```bash
ls -l backup/
```

and:

```bash
cat backup/important_file.txt
```

Result:

```text
Important backup data
```

---

## Existing Directory Test

The same command was executed again:

```bash
mkdir backup || echo "Backup directory already exists" && cp important_file.txt backup/
```

This time `mkdir` failed because the directory already existed.

Observed output included:

```text
mkdir: cannot create directory ‘backup’: File exists
Backup directory already exists
```

This demonstrated the fallback behavior of `||`.

---

## Important Operator Evaluation Note

The supplied command:

```bash
mkdir backup || echo "Backup directory already exists" && cp important_file.txt backup/
```

should not be interpreted as simply meaning:

> Create the directory, and only then copy the file.

In Bash, `&&` and `||` have equal precedence and are evaluated from left to right.

Therefore, when building a command where the copy operation must depend specifically on successful directory creation, the clearer form is:

```bash
mkdir backup && cp important_file.txt backup/
```

This version was tested successfully.

---

## Failure Test

To demonstrate the behavior of `&&`, a file named `backup` was created:

```bash
touch backup
```

Then:

```bash
mkdir backup && cp important_file.txt backup/
```

The `mkdir` command failed because `backup` was already a file:

```text
mkdir: cannot create directory ‘backup’: File exists
```

The `cp` command was not executed because the first command failed.

### Result

The case study demonstrated how command chaining can be used for:

* Conditional execution.
* Error handling.
* Fallback operations.
* Backup workflows.
* Reliable command-line automation.

### Evidence

* `evidence/case-study-backup.txt`

---

# Evidence and Verification

A complete terminal session was recorded using the Linux `script` command.

The session provides chronological evidence of the commands executed during the lab.

## Evidence Files

```text
evidence/
├── case-study-backup.txt
├── operator-comparison.txt
├── operator-summary.txt
├── task1-and-failure.txt
├── task1-and-success.txt
├── task2-or.txt
├── task3-semicolon.txt
└── terminal-session.txt
```

Each evidence file corresponds to a specific task or verification performed during the lab.

---

# Practical Applications

Command chaining is commonly used in Linux administration and automation.

### Directory Navigation

```bash
mkdir logs && cd logs
```

Creates a directory and enters it only if creation succeeds.

### Error Handling

```bash
cd /missing/path || echo "Path not found"
```

Displays an error message when navigation fails.

### Sequential Execution

```bash
command1; command2; command3
```

Runs all commands sequentially regardless of individual command success.

### Conditional Service Management

```bash
systemctl restart service && systemctl status service
```

Checks the service status only if the restart succeeds.

---

# Key Takeaways

1. `&&` is useful when the next operation depends on successful completion of the previous operation.
2. `||` is useful for fallback actions and basic error handling.
3. `;` runs commands sequentially without checking the previous command's success.
4. Linux commands return exit statuses that can be used to control command chains.
5. Combining `&&` and `||` requires understanding their evaluation behavior.
6. Clear command construction improves reliability in shell automation.

---

# Conclusion

This lab successfully demonstrated command chaining in a Linux environment using the `&&`, `||`, and `;` operators.

The practical tests confirmed that `&&` performs success-dependent execution, `||` performs failure-dependent fallback execution, and `;` executes commands sequentially regardless of previous command status.

The backup case study further demonstrated how command chaining can be applied to real-world Linux administration and automation tasks. The lab also highlighted the importance of understanding shell operator evaluation when combining multiple conditional operators.

All required tasks were completed and supporting evidence was captured in the `evidence/` directory.
