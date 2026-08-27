# Lab 42: Mounting and Unmounting

## 📌 Lab Overview

This lab introduces the fundamentals of **mounting and unmounting filesystems in Linux**. Mounting makes a filesystem accessible through a directory in the Linux filesystem hierarchy, while unmounting safely detaches it.

These operations are essential for managing additional disks, partitions, USB drives, filesystem images, and storage devices in Linux environments.

---

## 🎯 Objectives

By completing this lab, you will:

* Understand the concept of Linux mount points.
* Create and manage a mount point directory.
* Identify available disks and partitions using `lsblk`.
* Mount a filesystem to a directory.
* Verify that a filesystem has been successfully mounted.
* Safely unmount a filesystem.
* Understand common mounting and unmounting errors.
* Learn basic storage-management best practices.

---

## 🛠️ Prerequisites

Before starting this lab, you should have:

* Basic Linux command-line knowledge.
* Access to a Linux system.
* `sudo` privileges.
* Basic understanding of disks, partitions, and filesystems.
* A test partition, disk, or filesystem image that can safely be mounted.

> ⚠️ **Important:** Do not experiment with a production disk or partition. Always verify the device name before using `mount`, `umount`, `mkfs`, `fdisk`, or other disk-management commands.

---

## 📚 Key Concepts

### What is Mounting?

**Mounting** is the process of attaching a filesystem to a directory so that its contents become accessible through the Linux directory tree.

For example:

```text
/dev/sdb1
    ↓
/mnt/my_mount
```

After mounting, the files stored on `/dev/sdb1` can be accessed through:

```text
/mnt/my_mount
```

### What is a Mount Point?

A **mount point** is an existing directory where a filesystem is attached.

Common mount points include:

```text
/mnt
/media
/home
```

For temporary testing, `/mnt` is commonly used.

### What is Unmounting?

**Unmounting** safely detaches a filesystem from the Linux filesystem tree.

The command used is:

```bash
sudo umount /mnt/my_mount
```

---

# 🧪 Task 1: Create a Mount Point

## Step 1: Create the Directory

Create a directory that will be used as the mount point:

```bash
sudo mkdir -p /mnt/my_mount
```

The `-p` option ensures that the required parent directories exist.

---

## Step 2: Verify the Directory

Check that the directory was created:

```bash
ls -ld /mnt/my_mount
```

Expected output will look similar to:

```text
drwxr-xr-x ... /mnt/my_mount
```

---

# 🔍 Task 2: Identify a Partition

Before mounting anything, identify the available disks and partitions.

## Step 1: Display Block Devices

Run:

```bash
lsblk
```

Example:

```text
NAME        MAJ:MIN RM  SIZE RO TYPE MOUNTPOINTS
nvme0n1     259:0    0   20G  0 disk
├─nvme0n1p1
│           259:1    0   19G  0 part /
└─nvme0n1p2
            259:2    0    1G  0 part [SWAP]
nvme1n1     259:3    0    5G  0 disk
└─nvme1n1p1
            259:4    0    5G  0 part
```

Identify the test partition you intend to mount.

For example:

```text
/dev/nvme1n1p1
```

> ⚠️ Never blindly copy `/dev/sdb1` from an example. Device names vary between systems.

---

## Step 2: Display Filesystem Information

Use:

```bash
lsblk -f
```

This provides additional information such as:

* Filesystem type
* UUID
* Label
* Mount point

Example:

```text
NAME        FSTYPE FSVER LABEL UUID                                 MOUNTPOINTS
nvme1n1p1   ext4   1.0         xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx
```

You can also use:

```bash
sudo blkid
```

---

# 📦 Task 3: Mount the Filesystem

Once the correct partition has been identified, mount it to the mount point.

Example:

```bash
sudo mount /dev/nvme1n1p1 /mnt/my_mount
```

Replace `/dev/nvme1n1p1` with the actual test partition on your system.

---

## Step 1: Verify the Mount

Run:

```bash
mount | grep /mnt/my_mount
```

You can also use:

```bash
df -hT /mnt/my_mount
```

Example:

```text
Filesystem     Type  Size  Used Avail Use% Mounted on
/dev/nvme1n1p1 ext4  4.9G   24K  4.6G   1% /mnt/my_mount
```

This confirms that the filesystem is mounted.

---

## Step 2: Check the Mounted Filesystem with `findmnt`

Run:

```bash
findmnt /mnt/my_mount
```

This provides a clean view of the filesystem associated with the mount point.

Example:

```text
TARGET         SOURCE         FSTYPE OPTIONS
/mnt/my_mount  /dev/nvme1n1p1 ext4   rw,relatime
```

---

## Step 3: Access the Mounted Filesystem

Change into the mount point:

```bash
cd /mnt/my_mount
```

Check its contents:

```bash
ls -la
```

You can also create a test file if the filesystem is writable:

```bash
sudo touch /mnt/my_mount/test-file.txt
```

Verify:

```bash
ls -l /mnt/my_mount
```

---

# 🛑 Task 4: Unmount the Filesystem

Before unmounting, leave the mount point:

```bash
cd ~
```

Then run:

```bash
sudo umount /mnt/my_mount
```

> Note that the command is `umount`, not `unmount`.

---

## Step 1: Verify the Filesystem Is Unmounted

Run:

```bash
findmnt /mnt/my_mount
```

If nothing is returned, the filesystem is no longer mounted there.

You can also check:

```bash
df -hT
```

---

## Step 2: Verify the Mount Point Still Exists

Unmounting removes the filesystem attachment, **not the mount-point directory**.

Check:

```bash
ls -ld /mnt/my_mount
```

The directory should still exist.

If the directory is no longer required, it can be removed:

```bash
sudo rmdir /mnt/my_mount
```

---

# 🔎 Useful Verification Commands

The following commands are useful when working with mounted filesystems:

### List block devices

```bash
lsblk
```

### Show filesystem information

```bash
lsblk -f
```

### Show filesystem UUIDs

```bash
sudo blkid
```

### Display mounted filesystems

```bash
mount
```

### Display disk usage

```bash
df -h
```

### Display filesystem types

```bash
df -hT
```

### Display information about a specific mount

```bash
findmnt /mnt/my_mount
```

---

# ⚠️ Troubleshooting

## Error: `mount point does not exist`

Example:

```text
mount: /mnt/my_mount: mount point does not exist
```

Create the directory:

```bash
sudo mkdir -p /mnt/my_mount
```

Then retry the mount command.

---

## Error: `wrong fs type`

Example:

```text
wrong fs type, bad option, bad superblock
```

Check the filesystem type:

```bash
lsblk -f
```

or:

```bash
sudo blkid /dev/nvme1n1p1
```

If necessary, specify the filesystem type:

```bash
sudo mount -t ext4 /dev/nvme1n1p1 /mnt/my_mount
```

Only specify a filesystem type when you know it is correct.

---

## Error: `target is busy`

Example:

```text
umount: /mnt/my_mount: target is busy
```

This usually means a process or terminal is currently using the filesystem.

First leave the directory:

```bash
cd ~
```

Then retry:

```bash
sudo umount /mnt/my_mount
```

You can identify processes using the mount point with:

```bash
sudo lsof +D /mnt/my_mount
```

or:

```bash
sudo fuser -vm /mnt/my_mount
```

---

# 🔐 Security and Safety Considerations

When working with storage devices:

* Always verify the device name before mounting.
* Never run filesystem-formatting commands on an unknown device.
* Avoid modifying production filesystems during practice.
* Use a dedicated test disk or partition whenever possible.
* Unmount removable storage before physically disconnecting it.
* Ensure important data has been backed up before performing disk operations.

---

# 📝 Lab Verification Checklist

* [ ] Created `/mnt/my_mount`.
* [ ] Verified the mount-point directory.
* [ ] Used `lsblk` to identify available devices.
* [ ] Used `lsblk -f` to identify the filesystem type.
* [ ] Mounted the test filesystem.
* [ ] Verified the mount using `findmnt`.
* [ ] Verified disk usage using `df -hT`.
* [ ] Accessed the mounted filesystem.
* [ ] Successfully unmounted the filesystem.
* [ ] Verified that the filesystem was no longer mounted.

---

# 💼 Practical Case Study

### Scenario: Accessing an External USB Drive

Suppose a Linux administrator connects a USB storage device containing an `ext4` filesystem.

First, the administrator identifies the device:

```bash
lsblk -f
```

Suppose the USB partition is:

```text
/dev/sdb1
```

A mount point is created:

```bash
sudo mkdir -p /mnt/usb
```

The filesystem is mounted:

```bash
sudo mount /dev/sdb1 /mnt/usb
```

The administrator can now access the files:

```bash
ls -la /mnt/usb
```

After finishing the work, the administrator safely unmounts it:

```bash
sudo umount /mnt/usb
```

Only after the filesystem has been successfully unmounted should the USB device be physically disconnected.

---

# 🧠 Key Takeaways

* A **mount point** is a directory where a filesystem becomes accessible.
* `lsblk` is useful for identifying disks and partitions.
* `lsblk -f` displays filesystem information.
* `mount` attaches a filesystem to a directory.
* `findmnt` helps verify mounted filesystems.
* `df -hT` displays filesystem usage and type.
* `umount` safely detaches a filesystem.
* A filesystem should be unmounted before removing removable storage.
* Always verify the correct device before performing storage operations.

---

## 🏁 Conclusion

In this lab, you learned how Linux manages filesystems through mounting and unmounting. You created a mount point, identified a storage device, mounted its filesystem, verified access, and safely unmounted it.

These skills are fundamental for **Linux system administration, storage management, server administration, and DevOps environments**.

---

## 📂 Lab Structure

```text
Lab-42-Mounting-and-Unmounting/
└── README.md
```

---

**Lab Status:** ✅ Completed

**Skills Practiced:** `mkdir` · `lsblk` · `lsblk -f` · `blkid` · `mount` · `findmnt` · `df` · `umount`
