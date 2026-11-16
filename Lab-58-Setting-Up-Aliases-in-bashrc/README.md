# Lab 58: Setting Up Aliases in `.bashrc`

## 1. Introduction

In Linux, an alias is a shortcut used to represent a command or a group of commands.

Aliases make command-line work easier, faster, and more convenient. Instead of typing a long command repeatedly, we can create a short name for it.

For example:

```bash
alias ll='ls -alF'
```

After creating this alias, we can type `ll` instead of `ls -alF`.

## 2. What Is an Alias?

An alias is a custom shortcut for a command.

**Example:**

```bash
alias myip='hostname -I'
```

Now, instead of typing:

```bash
hostname -I
```

We can simply type:

```bash
myip
```

### Types of aliases

1. **Temporary alias:** Works only in the current shell session.
2. **Permanent alias:** Saved in a configuration file such as `~/.bashrc` and loaded in future interactive Bash sessions.

## 3. What Is `.bashrc`?

The `.bashrc` file is a hidden configuration file located in the user's home directory.

Location:

```text
/home/ubuntu/.bashrc
```

It contains Bash settings and commands that are loaded when an interactive non-login Bash shell starts.

It can contain:

- Aliases
- Environment variables
- Shell functions
- Command prompt settings
- Other Bash configurations

To view hidden files:

```bash
ls -a
```

To inspect alias-related lines:

```bash
grep -n alias ~/.bashrc
```

**Important:** Ubuntu's default `.bashrc` can also load `~/.bash_aliases` if that file exists.

## 4. Lab Environment

| Item | Details |
|---|---|
| Operating System | Ubuntu 24.04 LTS |
| Shell | Bash |
| User | ubuntu |
| Lab Number | 58 |
| Repository | Linux-Deep-Dive-Labs |

## 5. Task 1: Create a Temporary Alias

Create an alias:

```bash
alias ll='ls -alF'
```

Verify it:

```bash
type ll
```

Expected output:

```text
ll is aliased to `ls -alF'
```

### Explanation

- `alias` is the command used to create an alias.
- `ll` is the shortcut name.
- `ls -alF` is the original command.
- `type ll` checks how Bash interprets `ll`.

**Note:** This alias is temporary. It will not automatically remain available after the current shell session ends.

## 6. Task 2: Inspect the Existing `.bashrc`

Commands:

```bash
cd ~
ls -a | grep bashrc
grep -n alias ~/.bashrc
```

### Explanation

- `cd ~` takes us to the user's home directory.
- `ls -a` displays hidden and normal files.
- `grep bashrc` searches for the `.bashrc` filename.
- `grep -n alias ~/.bashrc` searches for alias-related lines and displays their line numbers.

### Findings

The existing Ubuntu `.bashrc` already contained aliases such as:

```bash
alias ll='ls -alF'
alias la='ls -A'
alias l='ls -CF'
```

It also contained color-related aliases for commands such as `ls` and `grep`.

The `.bashrc` file also checks whether `~/.bash_aliases` exists and loads it.

## 7. Task 3: Add Permanent Aliases

We added three aliases to `.bashrc`.

### Commands used

```bash
grep -q "# lab58 aliases" ~/.bashrc || cat >> ~/.bashrc <<'EOF'

# lab58 aliases
alias ll='ls -alF'
alias myip='hostname -I'
alias aptup='sudo apt update'
EOF
```

### Explanation of the command

- `grep -q` searches quietly for the marker.
- `# lab58 aliases` identifies our lab section.
- `||` runs the command on the right only if the search fails.
- `cat >> ~/.bashrc` appends new content to `.bashrc`.
- `EOF` marks the end of the input.

This guard helps prevent duplicate alias sections.

### Aliases created

| Alias | Original command | Purpose |
|---|---|---|
| `ll` | `ls -alF` | Detailed directory listing |
| `myip` | `hostname -I` | Display IP addresses |
| `aptup` | `sudo apt update` | Refresh package information |

### Verify the configuration

```bash
grep -n -A 4 "# lab58 aliases" ~/.bashrc
```

Actual configuration:

```bash
# lab58 aliases
alias ll='ls -alF'
alias myip='hostname -I'
alias aptup='sudo apt update'
```

**Important:** The `aptup` alias was created and verified but was not intentionally executed during this lab.

## 8. Task 4: Reload `.bashrc`

After adding aliases, we must reload the configuration.

Command:

```bash
source ~/.bashrc
```

### What does `source` do?

The `source` command reads and executes commands from a file in the current shell.

It allows configuration changes to take effect without closing and reopening the terminal.

### Verify all aliases

```bash
type ll myip aptup
```

Actual results:

```text
ll is aliased to `ls -alF'
myip is aliased to `hostname -I'
aptup is aliased to `sudo apt update'
```

All three aliases were successfully verified.

## 9. Task 5: Test the Aliases

### Test 1: Detailed directory listing

```bash
ll ~ | head -n 3
```

Explanation:

- `ll` displays detailed directory information.
- `~` represents the home directory.
- `|` sends command output to another command.
- `head -n 3` displays the first three lines.

Result: The detailed directory listing was displayed successfully.

### Test 2: Display IP addresses

```bash
myip
```

Actual output:

```text
172.31.10.186 172.17.0.1
```

Result: The alias successfully executed `hostname -I`.

## 10. Task 6: Test a Fresh Interactive Shell

Command:

```bash
bash -ic 'type ll'
```

Actual result:

```text
ll is aliased to `ls -alF'
```

### Explanation

- `bash` starts a new Bash shell.
- `-i` starts an interactive shell.
- `-c` executes the command supplied as a string.
- `type ll` checks the alias.

An interactive Bash shell reads `.bashrc`, so the permanent alias is available.

## 11. Task 7: Test a Fresh Login Shell

Command:

```bash
bash -lic 'type myip'
```

Actual result:

```text
myip is aliased to `hostname -I'
```

### Explanation

- `-l` starts a login shell.
- `-i` starts an interactive shell.
- `-c` executes the specified command.

On Ubuntu, the login configuration normally loads `.bashrc` through the user's profile configuration.

The test confirmed that the `myip` alias was available.

## 12. Task 8: Test a Non-Interactive Shell

Command:

```bash
bash -c 'type ll'
```

Actual result:

```text
bash: line 1: type: ll: not found
```

### Why did this happen?

This is expected behaviour.

A non-interactive Bash shell does not normally read `.bashrc`.

Therefore, aliases defined in `.bashrc` are not automatically available in this type of shell.

**Important distinction:**

| Shell type | Reads `.bashrc` normally? |
|---|---|
| Interactive non-login | Yes |
| Interactive login | Through the login profile on Ubuntu |
| Non-interactive | No |

## 13. Important Linux Commands Used

| Command | Purpose |
|---|---|
| `alias` | Create or display aliases |
| `type` | Identify how a command is interpreted |
| `ls -a` | Display hidden files |
| `grep` | Search text |
| `source` | Reload configuration in the current shell |
| `hostname -I` | Display assigned IP addresses |
| `head` | Display the beginning of output |
| `bash -i` | Start an interactive shell |
| `bash -l` | Start a login shell |
| `bash -c` | Execute a command string |

## 14. Troubleshooting Notes

If an alias does not work, check its definition:

```bash
type ll
```

List available aliases:

```bash
alias
```

Inspect alias definitions:

```bash
grep -n alias ~/.bashrc
```

Reload the configuration:

```bash
source ~/.bashrc
```

If the alias is not available in a non-interactive shell, remember that this is normally expected behaviour.

## 15. Interview Questions and Answers

**Q1. What is an alias in Linux?**

An alias is a shortcut for a command. It helps users execute frequently used commands with shorter names.

**Q2. What is the purpose of `.bashrc`?**

It stores Bash configuration settings that are loaded when an interactive non-login Bash shell starts.

**Q3. What is the difference between a temporary and permanent alias?**

A temporary alias exists only in the current shell session. A permanent alias is stored in a configuration file and can be loaded in future sessions.

**Q4. What does `source ~/.bashrc` do?**

It reloads the `.bashrc` configuration in the current shell.

**Q5. How do you check whether an alias exists?**

Use:

```bash
type aliasname
```

**Q6. What is the purpose of `hostname -I`?**

It displays the IP addresses assigned to the system.

**Q7. Why are aliases unavailable in a non-interactive Bash shell?**

Because non-interactive Bash does not normally load `.bashrc`.

**Q8. What does `grep -q` do?**

It searches quietly and does not display matching lines. Its exit status can be used in shell conditions.

**Q9. Why use a duplicate-prevention guard?**

It prevents the same lab alias section from being appended repeatedly to `.bashrc`.

**Q10. What is the difference between `bash -i` and `bash -l`?**

`-i` starts an interactive shell, while `-l` starts a login shell.

## 16. Evidence

Practical command results are saved in:

```text
evidence/aliases-results.txt
```

This file records the commands, verification results, and actual outputs from the lab.

## 17. Conclusion

In this lab, I learned how to create temporary and permanent Bash aliases in Ubuntu.

I inspected the existing `.bashrc` file, added three aliases, used a duplicate-prevention guard, and reloaded the configuration.

I verified the aliases by running commands and testing fresh interactive, login, and non-interactive shells.

This practical exercise improved my understanding of Bash configuration, command shortcuts, and shell startup behaviour.

**Lab 58 practical tasks completed and verified.**
