# 💾 Lab 41: Basic Disk Partitioning

> **Linux Storage Management | Disk Partitioning | Block Devices | cfdisk**

---

## 📌 Lab Overview

Disk partitioning is a fundamental Linux system-administration skill used to organize and manage storage devices.

In this lab, you will identify available disks and partitions, inspect partition information, create a small test partition using **`cfdisk`**, and safely remove the test partition afterward.

The lab focuses on practical storage-management skills while emphasizing the importance of **protecting existing data**.

---

## 🎯 Objectives

By completing this lab, you will learn how to:

* Understand the purpose of disk partitioning.
* Identify available block devices.
* List disks and existing partitions.
* Inspect partition tables using `fdisk`.
* Use `cfdisk` to create a partition.
* Delete an unnecessary test partition.
* Verify partition changes using Linux storage commands.
* Apply safe practices when modifying disks.

---

## 🛠️ Prerequisites

* Basic Linux command-line knowledge.
* Access to a Linux system.
* `sudo` or root privileges.
* Basic understanding of disks and partitions.
* A **test disk or virtual machine** is strongly recommended.

> ⚠️ **Important:** Disk partitioning is potentially destructive. Never experiment on a disk containing important data unless you fully understand the consequences and have verified backups.

---

# 🧠 1. Understanding Disk Partitioning

A physical or virtual disk can be divided into smaller logical sections called **partitions**.

A simplified storage layout looks like:

```text
Physical Disk
/dev/sda
│
├── /dev/sda1
│     └── Partition 1
│
├── /dev/sda2
│     └── Partition 2
│
└── Free Space
```

Partitions can later be formatted with a filesystem and mounted for use.

For example:

```text
Disk
 │
 ├── Partition
 │       ↓
 │   Filesystem
 │       ↓
 │   Mount Point
 │       ↓
 │    /data
 │
 └── Free Space
```

---

# 📚 2. Important Storage Concepts

| Term            | Meaning                                    |
| --------------- | ------------------------------------------ |
| Block device    | Device used for block-based storage        |
| Disk            | Physical or virtual storage device         |
| Partition       | Logical division of a disk                 |
| Partition table | Structure describing partitions on a disk  |
| Filesystem      | Structure used to store and organize files |
| Mount point     | Directory where a filesystem is attached   |
| `lsblk`         | Lists block devices                        |
| `fdisk`         | Displays/manages partition tables          |
| `cfdisk`        | Interactive partitioning utility           |

---

# 🧪 Task 1: List Available Disks and Partitions

## Step 1 — Open the Terminal

Open your Linux terminal.

---

## Step 2 — List Block Devices

Run:

```bash id="qg5p7v"
lsblk
```

This displays a tree of available block devices and their partitions.

### Example

```text id="r7x3b8"
NAME        MAJ:MIN RM  SIZE RO TYPE MOUNTPOINTS
nvme0n1     259:0    0   20G  0 disk
├─nvme0n1p1
│           259:1    0   19G  0 part /
└─nvme0n1p2
            259:2    0    1G  0 part [SWAP]
nvme1n1     259:3    0    5G  0 disk
```

### 🔎 What to Observe

Look for:

* Disk names
* Partition names
* Disk sizes
* Partition sizes
* Filesystem types
* Mount points

---

## Step 3 — Display Filesystem Information

A useful extension of `lsblk` is:

```bash id="3wh7rc"
lsblk -f
```

This can show:

* Filesystem type
* UUID
* Mount point
* Label

Example:

```text id="n8u0se"
NAME        FSTYPE FSVER LABEL UUID                                 MOUNTPOINTS
nvme0n1
└─nvme0n1p1 ext4         ...   ...                                  /
```

---

# 🧪 Task 2: Inspect a Disk with `fdisk`

Choose the **test disk** you intend to examine.

For example:

```bash id="m1a7cn"
sudo fdisk -l /dev/sda
```

On systems using NVMe devices, the disk may instead be:

```bash id="k5zj8e"
sudo fdisk -l /dev/nvme1n1
```

> ⚠️ Replace the device name with the actual **test disk** identified using `lsblk`.

---

## 🔎 Understanding `fdisk` Output

The output can provide information such as:

* Disk size
* Sector size
* Partition table type
* Partition numbers
* Start and end sectors
* Partition sizes
* Partition types

Example:

```text id="e9q8zz"
Disk /dev/nvme1n1: 5 GiB
Device           Start       End   Sectors  Size Type
/dev/nvme1n1p1    2048   ...       ...      500M Linux filesystem
```

---

# 🧪 Task 3: Open `cfdisk`

`cfdisk` provides an interactive interface for managing partitions.

Open it against the **test disk**:

```bash id="z6t2k4"
sudo cfdisk /dev/nvme1n1
```

Or, if your test disk is `/dev/sda`:

```bash id="q5k1l3"
sudo cfdisk /dev/sda
```

### ⚠️ Critical Safety Check

Before making any changes, confirm:

```text
Is this definitely the test disk?
```

Do **not** continue if the device contains your operating system or important data.

---

# 🧪 Task 4: Create a Small Test Partition

Inside `cfdisk`:

### Step 1 — Identify Free Space

Use the arrow keys to select:

```text
Free Space
```

---

### Step 2 — Select `New`

Choose:

```text
New
```

`cfdisk` will ask for the partition size.

For example:

```text
500M
```

This creates a small test partition.

---

### Step 3 — Select the Partition Type

For a basic Linux data partition, select the appropriate Linux filesystem/partition type presented by `cfdisk`.

The exact menu wording may vary depending on the partition table and `cfdisk` version.

---

### Step 4 — Review Before Writing

Before committing the changes, carefully verify:

```text
Disk → Correct
Partition size → Correct
Partition location → Correct
Existing partitions → Unchanged
```

---

### Step 5 — Write the Changes

Select:

```text
Write
```

When prompted, confirm by entering:

```text
yes
```

> ⚠️ **This is the point where the partition-table changes are committed.**

---

### Step 6 — Exit `cfdisk`

Select:

```text
Quit
```

---

# 🔄 Task 5: Verify the New Partition

After exiting `cfdisk`, run:

```bash id="2qv7e4"
lsblk
```

The new partition should appear under the test disk.

For example:

```text id="6d1l1y"
nvme1n1
└─nvme1n1p1   500M  part
```

You can also inspect the partition table:

```bash id="j0d0q7"
sudo fdisk -l /dev/nvme1n1
```

---

# 🧪 Task 6: Remove the Test Partition

Once you have verified the partitioning operation, remove the test partition.

Open `cfdisk` again:

```bash id="x8m0g9"
sudo cfdisk /dev/nvme1n1
```

---

## Step 1 — Select the Test Partition

Use the arrow keys to select the partition you created.

For example:

```text
/dev/nvme1n1p1
```

---

## Step 2 — Select `Delete`

Choose:

```text
Delete
```

The partition should now appear as free space.

---

## Step 3 — Review the Changes

Before writing, confirm that:

* The intended test partition is being deleted.
* No other partition has been selected.
* Important partitions remain unchanged.

---

## Step 4 — Write the Changes

Select:

```text
Write
```

Then confirm:

```text
yes
```

---

## Step 5 — Exit

Select:

```text
Quit
```

---

# 🔍 Task 7: Verify Partition Removal

Run:

```bash id="5evw3y"
lsblk
```

The test partition should no longer appear.

You can also verify using:

```bash id="g7j3f4"
sudo fdisk -l /dev/nvme1n1
```

The disk should now show the available free space rather than the deleted test partition.

---

# 🧠 Partitioning Workflow

The complete workflow used in this lab is:

```text id="0p6l4h"
        ┌───────────────────┐
        │   Identify Disk   │
        │      lsblk        │
        └─────────┬─────────┘
                  ↓
        ┌───────────────────┐
        │ Inspect Partition │
        │      fdisk        │
        └─────────┬─────────┘
                  ↓
        ┌───────────────────┐
        │ Open cfdisk       │
        └─────────┬─────────┘
                  ↓
        ┌───────────────────┐
        │ Select Free Space │
        └─────────┬─────────┘
                  ↓
        ┌───────────────────┐
        │ Create Partition  │
        └─────────┬─────────┘
                  ↓
        ┌───────────────────┐
        │ Review Carefully  │
        └─────────┬─────────┘
                  ↓
        ┌───────────────────┐
        │ Write Changes     │
        └─────────┬─────────┘
                  ↓
        ┌───────────────────┐
        │ Verify with lsblk │
        └───────────────────┘
```

---

# 🔐 Safety & Data Protection

Partitioning requires extra caution because changing a partition table incorrectly can make data inaccessible.

### Before modifying a disk:

* Identify the correct device.
* Confirm whether it contains the operating system.
* Confirm the device is intended for testing.
* Back up important data.
* Check mounted filesystems.
* Review the partition table carefully.
* Never blindly copy a device name from an example.

### Useful command

```bash id="1x8xg7"
lsblk -o NAME,SIZE,FSTYPE,TYPE,MOUNTPOINTS
```

This gives a clearer overview before making changes.

---

# ⚠️ Common Mistakes

## Mistake 1: Selecting the Wrong Disk

This is one of the most dangerous mistakes.

Always run:

```bash id="l7skm9"
lsblk
```

before opening `cfdisk`.

---

## Mistake 2: Writing Changes Without Reviewing

The `Write` operation commits partition-table changes.

Always review the layout first.

---

## Mistake 3: Modifying a Mounted System Partition

Do not casually modify partitions currently being used by the operating system.

---

## Mistake 4: Assuming Device Names

Different systems use different device naming conventions.

Examples include:

```text
/dev/sda
/dev/vda
/dev/nvme0n1
/dev/nvme1n1
```

Always identify the actual device on your system.

---

# 🚀 DevOps & Cloud Relevance

Storage management is an important DevOps and Linux administration skill.

In cloud environments, administrators may need to:

* Identify attached volumes.
* Create partitions.
* Prepare disks for applications.
* Configure filesystems.
* Mount additional storage.
* Troubleshoot disk capacity issues.
* Manage persistent storage.

A typical cloud-storage workflow may look like:

```text id="1m1u6w"
Cloud Volume
     │
     ▼
Linux Block Device
     │
     ▼
Partition
     │
     ▼
Filesystem
     │
     ▼
Mount Point
     │
     ▼
Application Data
```

Understanding the difference between a **disk, partition, filesystem, and mount point** is essential when working with Linux servers.

---

# 📊 Command Reference

| Command                  | Purpose                            |
| ------------------------ | ---------------------------------- |
| `lsblk`                  | List block devices                 |
| `lsblk -f`               | Display filesystem information     |
| `lsblk -o ...`           | Customize block-device output      |
| `sudo fdisk -l`          | List partition tables              |
| `sudo fdisk -l /dev/sda` | Inspect a specific disk            |
| `sudo cfdisk /dev/sda`   | Open interactive partitioning tool |
| `cfdisk` → `New`         | Create a partition                 |
| `cfdisk` → `Delete`      | Delete a partition                 |
| `cfdisk` → `Write`       | Commit partition-table changes     |
| `cfdisk` → `Quit`        | Exit the tool                      |

---

# 🧪 Lab Verification Checklist

### 1. Identify Available Disks

```bash id="3c6q8v"
lsblk
```

### 2. Display Filesystem Information

```bash id="0y3q4k"
lsblk -f
```

### 3. Inspect the Test Disk

```bash id="u1u8fv"
sudo fdisk -l /dev/nvme1n1
```

### 4. Open the Partitioning Tool

```bash id="n8k5m7"
sudo cfdisk /dev/nvme1n1
```

### 5. Create the Test Partition

Inside `cfdisk`:

```text
Free Space → New → 500M → Select Type → Write → yes → Quit
```

### 6. Verify the Partition

```bash id="j6t7t2"
lsblk
```

### 7. Delete the Test Partition

```bash id="l8q2h1"
sudo cfdisk /dev/nvme1n1
```

Then:

```text
Select Partition → Delete → Write → yes → Quit
```

### 8. Confirm Final State

```bash id="e2g5w8"
lsblk
```

```bash id="s8u4k1"
sudo fdisk -l /dev/nvme1n1
```

---

# 🎓 Learning Outcomes

After completing this lab, you should be able to:

* Explain what disk partitioning is.
* Identify Linux block devices.
* Inspect disk and partition information.
* Use `lsblk` to understand storage layouts.
* Use `fdisk` to inspect partition tables.
* Use `cfdisk` to create and remove partitions.
* Verify changes after partition operations.
* Recognize the risks associated with disk management.
* Apply safe storage-management practices.

---

# 🏁 Conclusion

In this lab, you gained hands-on experience with **Linux disk partitioning**.

You used:

```text
lsblk → Identify storage
fdisk → Inspect partition tables
cfdisk → Create/delete partitions
lsblk → Verify changes
```

The lab demonstrates an important Linux administration principle:

> **Always identify and verify the target disk before making storage changes.**

Disk partitioning is a foundational skill for **Linux administration, DevOps, cloud infrastructure, server management, and storage troubleshooting**.

---

## 📌 Key Takeaway

```text
       Identify
          ↓
        Inspect
          ↓
        Modify
          ↓
        Verify
          ↓
        Protect Data
```

> 💾 **Storage changes should never be performed blindly. Verify the device, understand the partition layout, make controlled changes, and confirm the final state.**

---

## 🏆 Lab Status

**Lab 41 — Basic Disk Partitioning**
**Status:** ✅ Completed
**Focus:** Linux Storage • Disk Management • Partitioning • `lsblk` • `fdisk` • `cfdisk` • DevOps
