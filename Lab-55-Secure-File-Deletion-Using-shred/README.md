# Lab 55: Secure File Deletion Using `shred`

## 1. Lab Overview

This lab demonstrates how to securely overwrite test files using the Linux `shred` command.

The practical was performed on Ubuntu 24.04 LTS using GNU coreutils 9.4.

## 2. Objectives

By completing this lab, I learned how to:

- Understand the difference between normal deletion and secure overwriting.
- Use the Linux `shred` command.
- Perform multiple overwrite passes.
- Verify overwritten file contents.
- Remove test files after the practical.
- Understand the limitations of secure file deletion.

## 3. What Is Secure File Deletion?

When we use the normal `rm` command, Linux removes the file's directory entry. This does not necessarily overwrite the data stored in the file's disk blocks.

The `shred` command attempts to overwrite file contents so that the original data is harder to recover.

**Important:** Secure deletion depends on the filesystem and storage device. `shred` cannot guarantee complete erasure on every system.

## 4. Understanding the `shred` Command

Basic syntax:

```bash
shred [options] filename
```

Important options:

| Option | Purpose |
|---|---|
| `-v` | Displays the overwrite progress |
| `-n 3` | Performs three random overwrite passes |
| `-z` | Adds a final pass of zeros |
| `-u` | Removes the file after overwriting |

### Difference Between `rm` and `shred`

| Command | Purpose |
|---|---|
| `rm filename` | Removes the file's directory entry |
| `shred filename` | Overwrites the file contents |
| `shred -u filename` | Overwrites and then removes the file |

## 5. Practical Tasks Performed

### Task 1: Create Test Files

Created a scratch directory and a sample file containing test text.

```bash
mkdir -p ~/shred-lab
cd ~/shred-lab

echo "This is a sensitive file that needs secure deletion." > sample.txt
cp sample.txt orig.txt
cat sample.txt
```

The original file was inspected using:

```bash
xxd sample.txt | head
```

The output showed the original text in hexadecimal form.

### Task 2: Overwrite the Sample File

Executed:

```bash
shred -v -n 3 -z sample.txt
```

The command performed four passes:

1. First random overwrite pass.
2. Second random overwrite pass.
3. Third random overwrite pass.
4. Final zero overwrite pass.

The `sample.txt` file remained available for verification because the `-u` option was not used.

### Task 3: Verify the Overwritten Contents

Inspected the file:

```bash
xxd sample.txt | head
```

The displayed bytes were zeros.

Compared the overwritten file with the original backup:

```bash
cmp sample.txt orig.txt || echo differs
```

The files differed at byte 1.

Checked whether the original keyword remained:

```bash
grep -c sensitive sample.txt
```

The result was `0`, meaning the keyword was not found.

### Task 4: Remove the Test Files

Removed the overwritten sample file:

```bash
rm sample.txt
```

Securely overwrote and removed the original backup:

```bash
shred -v -u orig.txt
```

Verified that both files no longer existed:

```bash
test ! -e sample.txt && echo "sample.txt removed"
test ! -e orig.txt && echo "orig.txt removed"
ls -la
```

Both removal checks succeeded.

### Task 5: Clean Up

Removed the empty scratch directory:

```bash
cd ~
rmdir ~/shred-lab
```

Verified that the directory was removed.

## 6. Important Limitations

The `shred` command is not a universal guarantee of secure data erasure.

It may not work as expected in situations involving:

- SSD wear levelling.
- Copy-on-write filesystems such as Btrfs and ZFS.
- Filesystem snapshots.
- Backups or other copies of the original data.
- Storage devices that do not overwrite data in place.

For sensitive information, full-disk encryption and suitable device-level secure-erase methods may provide better protection.

## 7. Safety Precautions

- Use only temporary test files when learning `shred`.
- Do not run destructive commands on important files.
- Check the filename and directory before using `shred -u`.
- Remember that secure deletion cannot be guaranteed on every storage device.

## 8. Evidence

Practical terminal results are saved in:

```text
evidence/shred-results.txt
```

The evidence includes:

- GNU `shred` version.
- Test file creation.
- Original file inspection.
- Overwrite progress.
- Hexadecimal inspection after overwriting.
- File comparison and keyword check.
- File removal verification.
- Scratch directory cleanup.

## 9. Conclusion

In this lab, I learned how to use the Linux `shred` command to overwrite test file contents before deletion.

I performed multiple overwrite passes, inspected the resulting contents, compared the overwritten file with its original copy, and verified file removal.

I also learned that secure deletion depends on the filesystem and storage technology.

**Lab Status:** Practical tasks completed and verified.
