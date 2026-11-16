# Lab 54: Using df and du

## Linux Disk Space and Directory Usage Management

**Environment:** Ubuntu 24.04 LTS
**User:** ubuntu
**Lab Category:** Linux System Administration
**Tools:** `df`, `du`, `sort`, `head`, `truncate`

---

## 1. Lab Objectives

The purpose of this lab was to understand how to monitor filesystem capacity and investigate disk usage in Linux.

By completing this lab, I practised:

- Checking available disk space using `df`.
- Displaying filesystem types.
- Checking inode usage.
- Inspecting the root filesystem.
- Measuring directory and file sizes using `du`.
- Identifying large files and directories.
- Understanding sparse files.
- Cleaning up temporary test data.

---

## 2. Understanding df

The `df` command stands for **Disk Free**.

It displays information about filesystem disk space, including total size, used space, available space, and percentage of usage.

### Task 1: Check Disk Space

Command:

```bash
df -h
```

Explanation:

- `df` displays filesystem disk usage.
- `-h` displays sizes in human-readable units such as KB, MB, and GB.

Important columns:

| Column | Description |
|---|---|
| Filesystem | Filesystem or device name |
| Size | Total filesystem size |
| Used | Space currently occupied |
| Avail | Available space |
| Use% | Percentage of space used |
| Mounted on | Filesystem mount location |

### Practical Result

The main root filesystem showed:

| Property | Result |
|---|---|
| Filesystem | /dev/nvme0n1p1 |
| Total size | 29 GB |
| Used space | 8.3 GB |
| Available space | 20 GB |
| Usage | 30% |
| Mount point | / |

**Observation:** The root filesystem had approximately 20 GB of available space.

---

## 3. Display Filesystem Types

Command:

```bash
df -hT -x tmpfs -x squashfs -x devtmpfs
```

Explanation:

- `-h` displays human-readable sizes.
- `-T` displays filesystem types.
- `-x` excludes specified filesystem types.

### Practical Result

| Filesystem | Type | Mount Point |
|---|---|---|
| /dev/nvme0n1p1 | ext4 | / |
| /dev/nvme0n1p16 | ext4 | /boot |
| /dev/nvme0n1p15 | vfat | /boot/efi |

**Observation:**

- `ext4` is a Linux filesystem.
- `vfat` is used for the EFI boot partition.
- Temporary filesystems were excluded from the output.

---

## 4. Check Inode Usage

An inode is a filesystem data structure that stores information about a file or directory, such as ownership, permissions, and data block locations.

A filesystem can run out of available inodes even when it still has free disk space.

Command:

```bash
df -i /
```

### Practical Result

| Property | Result |
|---|---:|
| Total inodes | 3,801,088 |
| Used inodes | 296,005 |
| Free inodes | 3,505,083 |
| Inode usage | 8% |

**Observation:** Only 8% of the available inodes were used.

---

## 5. Inspect the Root Filesystem

Command:

```bash
df -hT /
```

This command displays detailed disk usage information for the root filesystem.

### Practical Result

| Property | Result |
|---|---|
| Filesystem | /dev/nvme0n1p1 |
| Type | ext4 |
| Size | 29 GB |
| Used | 8.3 GB |
| Available | 20 GB |
| Usage | 30% |

**Important:** If filesystem usage approaches 90%, a system administrator should investigate large files, remove unnecessary data, or consider increasing storage capacity.

---

## 6. Understanding du

The `du` command stands for **Disk Usage**.

It measures the amount of disk space occupied by files and directories.

Unlike `df`, which reports filesystem capacity, `du` helps investigate how much space individual directories and files consume.

---

## 7. Create Sample Directories

Command:

```bash
mkdir -p ~/lab/Documents/reports/2024 ~/lab/Documents/media ~/lab/Documents/notes
```

This created the following directory structure:

```text
Documents/
├── media/
├── notes/
└── reports/
    └── 2024/
```

---

## 8. Create Sample Files

### Create a 50 MB file

```bash
dd if=/dev/zero of=~/lab/Documents/media/video.bin bs=1M count=50 status=none
```

### Create a 10 MB file

```bash
dd if=/dev/zero of=~/lab/Documents/media/photo.bin bs=1M count=10 status=none
```

### Create a 20 MB report file

```bash
dd if=/dev/zero of=~/lab/Documents/reports/2024/annual.bin bs=1M count=20 status=none
```

### Create a 5 MB sparse file

```bash
truncate -s 5M ~/lab/Documents/reports/summary.bin
```

### Create a notes file

```bash
echo "meeting notes" > ~/lab/Documents/notes/todo.txt
```

---

## 9. Verify the Directory Structure

Command:

```bash
ls -lR ~/lab/Documents
```

Explanation:

- `-l` displays detailed file information.
- `-R` lists directories recursively.

### Files Created

| File | Logical Size |
|---|---:|
| video.bin | 50 MB |
| photo.bin | 10 MB |
| annual.bin | 20 MB |
| summary.bin | 5 MB |
| todo.txt | 14 bytes |

The total logical file size was approximately 85 MB.

---

## 10. Check Total Directory Usage

Command:

```bash
du -sh ~/lab/Documents
```

Explanation:

- `-s` displays a summary.
- `-h` displays human-readable sizes.

### Practical Result

```text
81M     /home/ubuntu/lab/Documents
```

**Observation:** The directory occupied approximately 81 MB of disk space.

The result differs from the total logical file size because the sparse file did not allocate its full logical size on disk.

---

## 11. Understand Sparse Files

A sparse file is a file that has a logical size larger than the actual disk space allocated to it.

It can contain unallocated regions called holes.

### Check Logical File Size

```bash
ls -lh ~/lab/Documents/reports/summary.bin
```

Result:

```text
-rw-rw-r-- 1 ubuntu ubuntu 5.0M summary.bin
```

### Check Actual Disk Usage

```bash
du -h ~/lab/Documents/reports/summary.bin
```

Result:

```text
0       /home/ubuntu/lab/Documents/reports/summary.bin
```

### Analysis

| Command | Result |
|---|---|
| `ls -lh` | 5.0 MB |
| `du -h` | 0 |

The file has a logical size of 5 MB but no allocated data blocks.

This demonstrates why logical file size and actual disk usage are not always the same.

---

## 12. Check Individual Directory Usage

Command:

```bash
du -h --max-depth=1 ~/lab/Documents
```

Explanation:

`--max-depth=1` displays the usage of the immediate subdirectories and the total directory.

### Practical Result

```text
61M     /home/ubuntu/lab/Documents/media
8.0K    /home/ubuntu/lab/Documents/notes
21M     /home/ubuntu/lab/Documents/reports
81M     /home/ubuntu/lab/Documents
```

### Analysis

- `media` consumed approximately 61 MB.
- `reports` consumed approximately 21 MB.
- `notes` consumed approximately 8 KB.
- Total directory usage was approximately 81 MB.

The media directory consumed the most disk space.

---

## 13. Find the Largest Files and Directories

Command:

```bash
du -ah ~/lab/Documents | sort -rh | head -n 10
```

Explanation:

- `du -a` includes files and directories.
- `-h` displays human-readable sizes.
- `sort -r` sorts in reverse order.
- `sort -h` understands human-readable sizes.
- `head -n 10` displays the first ten entries.

### Practical Result

The largest entries included:

| Item | Usage |
|---|---:|
| Documents | 81 MB |
| media | 61 MB |
| video.bin | 50 MB |
| reports/2024 | 21 MB |
| reports | 21 MB |
| annual.bin | 20 MB |
| photo.bin | 10 MB |

### Display Only the Three Largest Entries

Command:

```bash
du -ah ~/lab/Documents | sort -rh | head -n 3
```

Result:

```text
81M     /home/ubuntu/lab/Documents
61M     /home/ubuntu/lab/Documents/media
50M     /home/ubuntu/lab/Documents/media/video.bin
```

**Observation:** The `video.bin` file was the largest individual file.

This technique is useful for finding storage-consuming files during Linux troubleshooting.

---

## 14. Cleanup Temporary Files

After completing the practical tasks, I removed the sample directory.

Command:

```bash
rm -rf ~/lab/Documents
```

Verification:

```bash
ls ~/lab
```

Result:

```text
```

The directory was empty after cleanup.

**Important:** Always verify the target path before using `rm -rf`, because this command permanently removes the specified directory and its contents.

---

## 15. Difference Between df and du

| Feature | df | du |
|---|---|---|
| Full form | Disk Free | Disk Usage |
| Main purpose | Filesystem capacity | File and directory usage |
| Shows available space | Yes | No |
| Shows used space | Yes | Yes, for selected paths |
| Checks inode usage | Yes, with `-i` | No |
| Finds large directories | Not directly | Yes |

### When to Use Each Command

Use `df` when:

- Checking available disk space.
- Monitoring filesystem capacity.
- Investigating a nearly full filesystem.
- Checking inode availability.

Use `du` when:

- Finding large directories.
- Checking individual file usage.
- Investigating storage consumption.
- Identifying files that may need cleanup.

---

## 16. Important Commands Reference

| Command | Purpose |
|---|---|
| `df -h` | Human-readable disk space |
| `df -hT` | Display filesystem types |
| `df -i /` | Check inode usage |
| `df -hT /` | Inspect root filesystem |
| `du -sh DIRECTORY` | Total directory usage |
| `du -h FILE` | File disk usage |
| `du -h --max-depth=1 DIRECTORY` | Immediate subdirectory usage |
| `du -ah DIRECTORY` | List files and directories with sizes |
| `sort -rh` | Sort human-readable sizes |
| `head -n 10` | Display ten entries |
| `truncate -s 5M FILE` | Set logical file size |
| `rm -rf DIRECTORY` | Remove directory and contents |

---

## 17. What I Learned

Through this practical lab, I learned:

1. How to check disk capacity using `df`.
2. How to identify filesystem types.
3. How to inspect inode availability.
4. How to measure directory sizes using `du`.
5. How to find the largest files and folders.
6. How sparse files work.
7. Why logical file size can differ from allocated disk usage.
8. How to remove temporary files safely after checking their contents.

---

## 18. Conclusion

This lab improved my understanding of Linux filesystem monitoring and disk usage analysis.

I used `df` to inspect filesystem capacity and inode usage. I used `du` to investigate directory sizes, identify large files, and compare logical file sizes with actual allocated disk space.

These commands are important for Linux system administration, disk monitoring, and troubleshooting storage problems.

**Status:** Practical tasks completed and terminal evidence recorded.
