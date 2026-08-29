# Lab 46: Bash Profile vs. Bashrc

## 📌 Lab Overview

This lab explores two important Bash configuration files: **`.bash_profile`** and **`.bashrc`**.

These files allow Linux users to customize their shell environment by configuring environment variables, aliases, functions, paths, and other shell behavior.

A key distinction is:

```text id="4p5m8j"
.bash_profile
      ↓
Login Shell

.bashrc
      ↓
Interactive Non-Login Shell
```

Understanding when each file is loaded is important for Linux administration, scripting, DevOps environments, and shell customization.

> **Note:** On many modern Linux distributions, including Ubuntu, `.bash_profile` may not exist by default. Instead, `.profile` is commonly used for login-shell configuration. `.bash_profile` is especially common in Bash setups on systems such as RHEL/CentOS or customized environments.

---

# 🎯 Objectives

By completing this lab, you will:

* Understand the purpose of `.bash_profile`.
* Understand the purpose of `.bashrc`.
* Learn the difference between login and non-login Bash shells.
* Locate Bash configuration files in the home directory.
* Inspect existing shell configuration.
* Create backups before modifying configuration files.
* Add aliases to `.bashrc`.
* Add environment variables to `.bashrc`.
* Reload `.bashrc` without logging out.
* Verify aliases and environment variables.
* Understand how `.bash_profile` can load `.bashrc`.
* Troubleshoot common shell configuration problems.

---

# 🛠️ Prerequisites

Before starting this lab, you should have:

* Basic Linux command-line knowledge.
* Access to a Linux system.
* A valid user home directory.
* Bash installed as the shell.
* Permission to modify your own home-directory configuration files.
* Familiarity with `nano`, `vim`, or another text editor.

Check your current shell:

```bash id="4r6m1s"
echo $SHELL
```

You can also check your current Bash version:

```bash id="x3j8y5"
bash --version
```

---

# 📚 Introduction to Bash Configuration

Bash reads specific configuration files depending on how the shell is started.

Two important files are:

```text id="j5w0qn"
~/.bash_profile
~/.bashrc
```

The `~` represents the user's home directory.

For example:

```text id="a7f2kd"
/home/ubuntu/.bashrc
```

---

# 🔑 `.bash_profile` vs `.bashrc`

| File            | Main Purpose                    | Typically Used By            |
| --------------- | ------------------------------- | ---------------------------- |
| `.bash_profile` | Login-shell configuration       | Login shells                 |
| `.bashrc`       | Interactive shell configuration | Interactive non-login shells |

### `.bash_profile`

Used primarily for **login shells**.

Typical configuration may include:

* Environment variables.
* `PATH` modifications.
* Startup programs.
* Commands that should run when logging into the system.

### `.bashrc`

Used primarily for **interactive non-login shells**.

Typical configuration includes:

* Aliases.
* Shell functions.
* Prompt customization.
* Interactive shell settings.
* Environment-related configuration.

---

# 🧠 Login Shell vs. Non-Login Shell

## Login Shell

A login shell is started when a user logs into the system.

For example:

```text id="w6s2k8"
SSH Login
    ↓
Login Shell
    ↓
Login configuration
```

Example:

```bash id="e3r7q2"
ssh username@server
```

---

## Interactive Non-Login Shell

A new terminal shell inside an existing graphical or shell session is commonly an interactive non-login shell.

Example:

```bash id="k8p5r0"
bash
```

This commonly reads:

```text id="n7v2y4"
~/.bashrc
```

---

# 🔍 Task 1: Navigate to the Home Directory

Move to your home directory:

```bash id="x4m6n1"
cd ~
```

Verify your current location:

```bash id="p2q8s7"
pwd
```

Example:

```text id="4h1z3k"
/home/ubuntu
```

---

# 📂 Task 2: Find Bash Configuration Files

List all files, including hidden files:

```bash id="v5n9c2"
ls -la
```

Look for:

```text id="d7m4q1"
.bashrc
.bash_profile
.profile
```

> **Important:** Do not worry if `.bash_profile` does not exist. Many Ubuntu systems use `.profile` for login-shell configuration instead.

You can check specifically:

```bash id="f6k2w9"
ls -la ~/.bash_profile ~/.bashrc ~/.profile 2>/dev/null
```

---

# 🔎 Task 3: Check Which Shell You Are Using

Run:

```bash id="s1p7x4"
echo $SHELL
```

Example:

```text id="n3c8v6"
/bin/bash
```

Check the current shell process:

```bash id="m5r9q2"
ps -p $$ -o args=
```

This shows the command associated with your current shell.

---

# 📖 Task 4: Examine `.bashrc`

Display the contents:

```bash id="g2v6k8"
cat ~/.bashrc
```

For easier reading:

```bash id="r7q1m5"
less ~/.bashrc
```

Or open it for editing:

```bash id="w4n8c3"
nano ~/.bashrc
```

Look for existing:

* Aliases.
* Functions.
* Environment variables.
* Prompt configuration.
* Conditional statements.

---

# 📖 Task 5: Examine `.bash_profile`

First check whether it exists:

```bash id="t5k9p2"
ls -l ~/.bash_profile
```

If it exists:

```bash id="q3m7v1"
cat ~/.bash_profile
```

Or:

```bash id="j8x4c6"
nano ~/.bash_profile
```

If you receive:

```text id="z2d7n4"
No such file or directory
```

that is normal on many Ubuntu installations.

Check `.profile` instead:

```bash id="b6w1r9"
cat ~/.profile
```

---

# 🔗 Task 6: Understand the `.bash_profile` → `.bashrc` Relationship

A common configuration pattern is for `.bash_profile` to load `.bashrc`.

For example:

```bash id="k9p4s2"
if [ -f ~/.bashrc ]; then
    . ~/.bashrc
fi
```

This means:

```text id="w3m8q6"
Login Shell
     │
     ▼
.bash_profile
     │
     ▼
.bashrc
     │
     ▼
Aliases + Functions + Interactive Settings
```

This allows interactive Bash settings in `.bashrc` to also be available after login.

On Ubuntu, `.profile` commonly performs a similar role.

---

# 💾 Task 7: Back Up Your Configuration Files

Before making changes, create backups.

For `.bashrc`:

```bash id="p6t2y8"
cp ~/.bashrc ~/.bashrc.backup
```

If `.bash_profile` exists:

```bash id="c4m9r1"
cp ~/.bash_profile ~/.bash_profile.backup
```

If you are using `.profile`:

```bash id="n7v3k5"
cp ~/.profile ~/.profile.backup
```

Verify:

```bash id="h2q8w4"
ls -la ~/.bashrc*
```

---

# ⚙️ Task 8: Add an Alias to `.bashrc`

An alias creates a shortcut for a command.

Open `.bashrc`:

```bash id="r5m1x9"
nano ~/.bashrc
```

Add the following at the end:

```bash id="v8q3k6"
alias ll='ls -la'
```

Save and exit Nano:

```text id="x4p7m2"
Ctrl + O
Enter
Ctrl + X
```

---

# 🔄 Task 9: Reload `.bashrc`

Changes to `.bashrc` do not automatically affect the current shell.

Reload the configuration:

```bash id="j3n6w8"
source ~/.bashrc
```

You can also use:

```bash id="s9k2p5"
. ~/.bashrc
```

These commands perform the same basic operation.

---

# ✅ Task 10: Verify the Alias

Run:

```bash id="c7v4m1"
ll
```

The command should behave similarly to:

```bash id="h8q2z6"
ls -la
```

Verify that the alias exists:

```bash id="q5n9r3"
alias ll
```

Expected output:

```text id="m4x7p1"
alias ll='ls -la'
```

You can also check:

```bash id="w6k3t8"
type ll
```

---

# 🌎 Task 11: Add an Environment Variable

Open `.bashrc`:

```bash id="f2r8m5"
nano ~/.bashrc
```

Add:

```bash id="z7q4n1"
export MY_VAR="Hello World!"
```

Save and exit.

---

# 🔄 Task 12: Reload the Configuration

Run:

```bash id="a3m8v6"
source ~/.bashrc
```

---

# ✅ Task 13: Verify the Environment Variable

Run:

```bash id="y5k1p9"
echo "$MY_VAR"
```

Expected output:

```text id="b4n7q2"
Hello World!
```

Check whether the variable is exported:

```bash id="t8m3x6"
printenv MY_VAR
```

Expected:

```text id="r6q9w2"
Hello World!
```

---

# 🧪 Task 14: Verify Variable Inheritance

Because `MY_VAR` was exported, child processes can inherit it.

Run:

```bash id="k4p7n1"
bash
```

Then:

```bash id="q8m2v5"
echo "$MY_VAR"
```

Expected:

```text id="s3x6r9"
Hello World!
```

Exit the child shell:

```bash id="w5n8k2"
exit
```

This demonstrates the difference between a normal shell variable and an exported environment variable.

---

# 🧪 Task 15: Test a Login Shell

You can start a Bash login shell for testing:

```bash id="m7q3x1"
bash --login
```

Check:

```bash id="v4n9k6"
echo "$MY_VAR"
```

If your login configuration loads `.bashrc`, the variable should be available.

Check the alias:

```bash id="p2w8r5"
alias ll
```

Exit:

```bash id="c6m1z7"
exit
```

> The exact behavior depends on whether your login configuration (`.bash_profile`, `.profile`, etc.) sources `.bashrc`.

---

# 🔬 Task 16: Compare Login and Non-Login Shells

You can use Bash flags to test shell behavior.

Start an interactive non-login shell:

```bash id="j9x4q2"
bash
```

Exit:

```bash id="n5m7v3"
exit
```

Start an interactive login shell:

```bash id="r8k2p6"
bash --login
```

Exit:

```bash id="w3q9m1"
exit
```

This helps demonstrate that Bash uses different startup files depending on how the shell is launched.

---

# 📊 Startup File Summary

A simplified model is:

```text id="a6v2n8"
                    Bash
                     │
             ┌───────┴────────┐
             │                │
          Login          Non-Login
          Shell             Shell
             │                │
       .bash_profile        .bashrc
       or .profile
             │
             ▼
          .bashrc
      (if sourced by login
       configuration)
```

Remember:

```text id="m8p4q1"
.bash_profile → Login Shell
.bashrc       → Interactive Non-Login Shell
.profile      → Common login configuration on Ubuntu
```

---

# 🛠️ Useful Commands

| Command             | Purpose                      |
| ------------------- | ---------------------------- |
| `cd ~`              | Go to home directory         |
| `pwd`               | Display current directory    |
| `ls -la`            | Show hidden files            |
| `cat ~/.bashrc`     | Display `.bashrc`            |
| `nano ~/.bashrc`    | Edit `.bashrc`               |
| `source ~/.bashrc`  | Reload `.bashrc`             |
| `alias`             | Display aliases              |
| `type ll`           | Show what `ll` represents    |
| `echo $MY_VAR`      | Display environment variable |
| `printenv MY_VAR`   | Display exported variable    |
| `echo $SHELL`       | Show user's default shell    |
| `ps -p $$ -o args=` | Show current shell           |
| `bash`              | Start a new Bash shell       |
| `bash --login`      | Start a login Bash shell     |

---

# ⚠️ Troubleshooting

## Problem: `.bash_profile` Does Not Exist

On Ubuntu, this is normal.

Check:

```bash id="q6n2v8"
ls -la ~/.bash_profile ~/.profile 2>/dev/null
```

If `.profile` exists, inspect it:

```bash id="m4x9k1"
cat ~/.profile
```

---

## Problem: Alias Does Not Work

If `ll` is not recognized after editing `.bashrc`, run:

```bash id="r7p3c5"
source ~/.bashrc
```

Then:

```bash id="t2n8q4"
alias ll
```

If necessary, verify that the alias was actually added:

```bash id="w9m5x2"
grep -n "alias ll" ~/.bashrc
```

---

## Problem: Environment Variable Is Empty

Run:

```bash id="k3q8v6"
echo "$MY_VAR"
```

If nothing appears, check:

```bash id="j5n1r9"
grep -n "MY_VAR" ~/.bashrc
```

Then reload:

```bash id="p7m4x2"
source ~/.bashrc
```

---

## Problem: `.bashrc` Contains an Error

If sourcing `.bashrc` produces an error:

```bash id="v8c2m6"
source ~/.bashrc
```

Restore the backup:

```bash id="q4n7x1"
cp ~/.bashrc.backup ~/.bashrc
```

Then reload:

```bash id="z6m3p8"
source ~/.bashrc
```

---

# 🔐 Best Practices

When modifying Bash configuration:

* Always create a backup before making changes.
* Keep aliases simple and understandable.
* Avoid placing unnecessary commands in `.bashrc`.
* Do not put passwords or secrets directly into shell configuration files.
* Use `export` when a variable needs to be inherited by child processes.
* Test changes with `source ~/.bashrc`.
* Check syntax before making major configuration changes.
* Keep login configuration separate from interactive shell configuration where appropriate.
* Document custom configuration used in production environments.

---

# 🧪 Configuration Verification

After completing the lab, run:

```bash id="x7m2q9"
echo $SHELL
```

```bash id="c5n8v1"
alias ll
```

```bash id="h4r6k2"
echo "$MY_VAR"
```

```bash id="p9w3m7"
printenv MY_VAR
```

And:

```bash id="j6q1x8"
grep -n "alias ll" ~/.bashrc
```

```bash id="v2m5c9"
grep -n "MY_VAR" ~/.bashrc
```

These commands provide evidence that the configuration was successfully added.

---

# 📋 Lab Verification Checklist

* [ ] Navigated to the home directory.
* [ ] Listed hidden files with `ls -la`.
* [ ] Checked for `.bash_profile`.
* [ ] Checked for `.bashrc`.
* [ ] Checked `.profile` where applicable.
* [ ] Identified the current shell.
* [ ] Reviewed `.bashrc`.
* [ ] Reviewed `.bash_profile` or `.profile`.
* [ ] Created configuration backups.
* [ ] Added the `ll` alias.
* [ ] Reloaded `.bashrc`.
* [ ] Successfully tested `ll`.
* [ ] Added `MY_VAR`.
* [ ] Reloaded `.bashrc`.
* [ ] Verified `MY_VAR`.
* [ ] Tested environment-variable inheritance.
* [ ] Tested login and non-login Bash shells.
* [ ] Verified the final configuration.

---

# 💼 Practical Case Study

### Scenario: Improving a DevOps Administrator's Shell Environment

A Linux/DevOps administrator frequently uses:

```bash
ls -la
git status
```

Typing these commands repeatedly is inefficient.

The administrator adds useful aliases to `.bashrc`:

```bash id="e3m7q1"
alias ll='ls -la'
alias gs='git status'
```

They also define an environment variable:

```bash id="k8p2v5"
export PROJECT_DIR="$HOME/projects"
```

After saving the file, they reload it:

```bash id="w4n9x6"
source ~/.bashrc
```

Now:

```bash id="ll"
ll
```

runs:

```bash id="x5q7m2"
ls -la
```

and:

```bash id="r3v8k1"
gs
```

runs:

```bash id="n6p4w9"
git status
```

The administrator can also use:

```bash id="q2m7x5"
cd "$PROJECT_DIR"
```

This demonstrates how Bash configuration can improve productivity in everyday Linux and DevOps workflows.

---

# 🧠 Key Takeaways

### `.bash_profile`

Primarily associated with:

```text id="j4q8m2"
Login Shells
```

### `.bashrc`

Primarily associated with:

```text id="v7n3x6"
Interactive Non-Login Shells
```

### `.profile`

Commonly used for login configuration on Ubuntu and other Debian-based systems.

### `source`

Reloads a configuration file into the current shell:

```bash id="c9m5r1"
source ~/.bashrc
```

### `alias`

Creates command shortcuts:

```bash id="z2p6k8"
alias ll='ls -la'
```

### `export`

Makes variables available to child processes:

```bash id="h5q9v3"
export MY_VAR="Hello World!"
```

---

# 🏁 Conclusion

In this lab, you explored Bash startup and configuration files and learned how Linux determines which configuration files to read depending on the type of shell being started.

You examined `.bash_profile`, `.bashrc`, and `.profile`, created backups, added a custom alias and environment variable, reloaded the shell configuration, and verified the changes.

The most important concept is:

```text id="m3x7q9"
Login Shell
     ↓
.bash_profile / .profile
     ↓
.bashrc (when sourced)
```

while:

```text id="p8v2k5"
Interactive Non-Login Shell
     ↓
.bashrc
```

Understanding these files provides a strong foundation for **Linux administration, shell scripting, automation, DevOps, and system customization**.

---

## 📂 Lab Structure

```text id="r6n1w4"
Lab-46-Bash-Profile-vs-Bashrc/
└── README.md
```

---

**Lab Status:** ✅ Completed

**Skills Practiced:** `.bash_profile` · `.bashrc` · `.profile` · `source` · `alias` · `export` · `printenv` · `bash --login` · Shell Configuration
