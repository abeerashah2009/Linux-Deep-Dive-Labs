# Lab 51: Basic Regex with grep

## Introduction
This lab demonstrates how to use Regular Expressions (Regex) with the Linux `grep` command to search and filter text files.

## Objectives
- Understand basic Regular Expressions.
- Search text files using `grep`.
- Find lines starting and ending with specific patterns.
- Match optional characters using Basic and Extended Regex.

## Prerequisites
- Ubuntu Linux
- GNU grep 3.11
- Basic command-line knowledge

## Task 1: Search for Lines Starting with a Pattern

Created a sample file:

```bash
printf '%s\n' apple banana cherry apricot grapefruit > example.txt
cat example.txt
```

Search for lines beginning with `a`:

```bash
grep '^a' example.txt
```

Output:
```text
apple
apricot
```

Count matching lines:

```bash
grep -c '^a' example.txt
```

Output: `2`

**Explanation:** The `^` symbol matches the beginning of a line.

## Task 2: Search for Lines Ending with a Pattern

Search for lines ending with `y`:

```bash
grep 'y$' example.txt
```

Output:
```text
cherry
```

Count matching lines:

```bash
grep -c 'y$' example.txt
```

Output: `1`

Check the matching line number:

```bash
grep -n 'y$' example.txt
```

Output:
```text
3:cherry
```

**Explanation:** The `$` symbol matches the end of a line.

## Task 3: Filter Lines with Optional Characters

Created another sample file:

```bash
printf '%s\n' color colour colr > colourExample.txt
cat colourExample.txt
```

### Extended Regular Expression

```bash
grep -E 'colou?r' colourExample.txt
```

Output:
```text
color
colour
```

Count matches:

```bash
grep -cE 'colou?r' colourExample.txt
```

Output: `2`

**Explanation:** In Extended Regex, `?` makes the preceding character optional. Therefore, `u` may appear zero or one time.

### Basic Regular Expression

```bash
grep 'colou?r' colourExample.txt
echo "exit code: $?"
```

The command returned exit code `1` because `?` is not an optional-character operator in standard Basic Regex.

Use the escaped form:

```bash
/usr/bin/grep -G 'colou\?r' colourExample.txt
```

Output:
```text
color
colour
```

**Explanation:** In Basic Regex, `\?` makes the preceding character optional.

**Note:** An `awk` warning appeared after the Basic Regex command in this lab environment. The expected matches were still displayed correctly.

## Important Commands

| Command | Purpose |
|---|---|
| `grep '^a' file` | Find lines starting with a |
| `grep 'y$' file` | Find lines ending with y |
| `grep -n pattern file` | Display matching line numbers |
| `grep -c pattern file` | Count matching lines |
| `grep -E pattern file` | Use Extended Regex |
| `grep -G pattern file` | Use Basic Regex |

## Conclusion

Successfully completed all three practical tasks in Lab 51.

Learned how to:
- Match the beginning of a line using `^`.
- Match the end of a line using `$`.
- Count matching lines with `-c`.
- Display line numbers with `-n`.
- Use optional characters with Basic and Extended Regex.
