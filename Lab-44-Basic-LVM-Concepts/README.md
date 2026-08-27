# Lab 44: Basic LVM Concepts

## 📌 Lab Overview

This lab introduces the fundamentals of **Logical Volume Manager (LVM)** in Linux. LVM provides a flexible method of managing storage by adding an abstraction layer between physical storage devices and filesystems.

Unlike traditional fixed partitions, LVM allows administrators to create logical volumes that can be resized, extended, reduced, moved, and removed with greater flexibility.

In this lab, you will identify existing LVM components, create a physical volume, build a volume group, create and format a logical volume, mount it for testing, and safely remove the LVM configuration.

---

## 🎯 Objectives

By completing this lab, you will:

* Understand the basic architecture of LVM.
* Identify physical volumes, volume groups, and logical volumes.
* Inspect existing LVM configuration.
* Initialize a disk as a Physical Volume (PV).
* Create a Volume Group (VG).
* Create a Logical Volume (LV).
* Format an LV with the `ext4` filesystem.
* Mount and verify the logical volume.
* Safely unmount and remove an LV.
* Remove a volume group and physical volume.
* Understand the importance of storage safety when performing LVM operations.

---

## 🛠️ Prerequisites

Before starting this lab, ensure that you have:

* Basic Linux command-line knowledge.
* A Linux system such as Ubuntu or RHEL.
* `sudo` or root privileges.
* Basic knowledge of disks and filesystems.
* An **additional test disk or partition** available for LVM.
* Sufficient free storage for creating the required logical volume.

> ⚠️ **IMPORTANT:** LVM commands can destroy existing data if they are executed against the wrong disk. Never use a production disk for this lab. Carefully verify the device name before running `pvcreate`, `vgcreate`, `lvcreate`, `mkfs`, or `pvremove`.

---

# 📚 Introduction to LVM

## What Is LVM?

**Logical Volume Manager (LVM)** is a Linux storage-management technology that provides flexible management of disk space.

Traditional partitioning looks approximately like:

```text
Physical Disk
     │
     ├── Partition 1
     ├── Partition 2
     └── Partition 3
```

LVM introduces additional layers:

```text
Physical Disk
      │
      ▼
Physical Volume (PV)
      │
      ▼
Volume Group (VG)
      │
      ▼
Logical Volume (LV)
      │
      ▼
Filesystem
      │
      ▼
Mount Point
```

This separation makes storage management much more flexible.

---

# 🧩 Important LVM Components

## 1. Physical Volume (PV)

A **Physical Volume** is a disk or partition prepared for use by LVM.

Example:

```text
/dev/sdb
```

It is initialized with:

```bash
sudo pvcreate /dev/sdb
```

---

## 2. Volume Group (VG)

A **Volume Group** is a pool of storage created from one or more physical volumes.

Example:

```text
myvg
```

Created using:

```bash
sudo vgcreate myvg /dev/sdb
```

A volume group provides the available storage from which logical volumes can be created.

---

## 3. Logical Volume (LV)

A **Logical Volume** is a virtual block device created inside a volume group.

Example:

```text
mylv
```

Created using:

```bash
sudo lvcreate -L 1G -n mylv myvg
```

The logical volume can then be formatted with a filesystem and mounted like a normal partition.

---

# 🔍 Task 1: Check Existing LVM Configuration

Before making changes, inspect the current LVM configuration.

## Step 1: Display Logical Volumes

Run:

```bash
sudo lvdisplay
```

This displays detailed information about logical volumes.

A shorter command is:

```bash
sudo lvs
```

---

## Step 2: Display Volume Groups

Run:

```bash
sudo vgdisplay
```

Or:

```bash
sudo vgs
```

These commands show existing volume groups and their available space.

---

## Step 3: Display Physical Volumes

Run:

```bash
sudo pvdisplay
```

Or:

```bash
sudo pvs
```

These commands display physical volumes currently configured for LVM.

---

## Step 4: View the Storage Layout

Use:

```bash
lsblk
```

For filesystem and UUID information:

```bash
lsblk -f
```

These commands help you understand the relationship between disks, partitions, filesystems, and mount points.

---

# 💽 Task 2: Identify the Test Disk

Before creating an LVM physical volume, identify the additional storage device.

Run:

```bash
sudo fdisk -l
```

You can also use:

```bash
lsblk -o NAME,SIZE,TYPE,FSTYPE,MOUNTPOINTS
```

Example:

```text
NAME        SIZE TYPE FSTYPE MOUNTPOINTS
nvme0n1      20G disk
├─nvme0n1p1  19G part ext4   /
└─nvme0n1p2   1G part swap  [SWAP]
nvme1n1       5G disk
```

In this example:

```text
/dev/nvme1n1
```

could be the additional test disk.

> ⚠️ Your system may use a different device name such as `/dev/sdb`, `/dev/vdb`, or `/dev/nvme1n1`. **Do not assume `/dev/sdb` exists.**

---

# 🧱 Task 3: Create a Physical Volume

## Step 1: Initialize the Disk

Assuming the test disk is:

```text
/dev/sdb
```

initialize it:

```bash
sudo pvcreate /dev/sdb
```

Example output:

```text
Physical volume "/dev/sdb" successfully created.
```

This prepares the disk for use by LVM.

---

## Step 2: Verify the Physical Volume

Run:

```bash
sudo pvs
```

You may see:

```text
PV         VG   Fmt  Attr PSize  PFree
/dev/sdb        lvm2 ---  5.00g  5.00g
```

For detailed information:

```bash
sudo pvdisplay /dev/sdb
```

---

# 🏗️ Task 4: Create a Volume Group

Create a volume group called `myvg`:

```bash
sudo vgcreate myvg /dev/sdb
```

Example output:

```text
Volume group "myvg" successfully created
```

---

## Verify the Volume Group

Run:

```bash
sudo vgs
```

Example:

```text
VG   #PV #LV #SN Attr   VSize VFree
myvg   1   0   0 wz--n- 5.00g 5.00g
```

For detailed information:

```bash
sudo vgdisplay myvg
```

The available space in the volume group can now be allocated to logical volumes.

---

# 💾 Task 5: Create a Logical Volume

Create a 1 GB logical volume named `mylv`:

```bash
sudo lvcreate -L 1G -n mylv myvg
```

Example output:

```text
Logical volume "mylv" created.
```

---

## Verify the Logical Volume

Run:

```bash
sudo lvs
```

You can also use:

```bash
sudo lvdisplay /dev/myvg/mylv
```

The logical volume should be available as:

```text
/dev/myvg/mylv
```

It may also be accessible through:

```text
/dev/mapper/myvg-mylv
```

---

# 🧹 Task 6: Format the Logical Volume

Before storing files on the logical volume, create a filesystem on it.

For this lab, use `ext4`:

```bash
sudo mkfs.ext4 /dev/myvg/mylv
```

Example output will show the creation of the ext4 filesystem.

> ⚠️ `mkfs.ext4` destroys existing data on the specified logical volume. Make sure you are formatting the correct test LV.

---

## Verify the Filesystem

Run:

```bash
lsblk -f
```

You should see `ext4` associated with the logical volume.

You can also run:

```bash
sudo blkid /dev/myvg/mylv
```

---

# 📁 Task 7: Create a Mount Point

Create a directory for the logical volume:

```bash
sudo mkdir -p /mnt/mylv
```

Verify:

```bash
ls -ld /mnt/mylv
```

---

# 🔗 Task 8: Mount the Logical Volume

Mount the logical volume:

```bash
sudo mount /dev/myvg/mylv /mnt/mylv
```

---

## Verify the Mount

Run:

```bash
findmnt /mnt/mylv
```

You can also use:

```bash
df -hT /mnt/mylv
```

Example:

```text
Filesystem           Type  Size  Used Avail Use% Mounted on
/dev/mapper/myvg-mylv
                     ext4  974M   24K  907M   1% /mnt/mylv
```

---

# 🧪 Task 9: Test the Logical Volume

Create a test file:

```bash
sudo touch /mnt/mylv/lvm-test.txt
```

Add some information:

```bash
echo "LVM Lab 44 Test File" | sudo tee /mnt/mylv/lvm-test.txt
```

Verify:

```bash
cat /mnt/mylv/lvm-test.txt
```

Expected output:

```text
LVM Lab 44 Test File
```

Check disk usage:

```bash
df -h /mnt/mylv
```

This confirms that the logical volume is mounted and usable.

---

# 🛑 Task 10: Safely Remove the Logical Volume

After testing, the LVM configuration can be removed.

The correct order is important:

```text
Unmount
   ↓
Remove Logical Volume
   ↓
Remove Volume Group
   ↓
Remove Physical Volume
```

---

## Step 1: Leave the Mount Point

If your terminal is currently inside `/mnt/mylv`, move somewhere else:

```bash
cd ~
```

---

## Step 2: Unmount the Logical Volume

Run:

```bash
sudo umount /mnt/mylv
```

Verify:

```bash
findmnt /mnt/mylv
```

No output indicates that the filesystem is no longer mounted.

---

## Step 3: Remove the Logical Volume

Remove the LV:

```bash
sudo lvremove /dev/myvg/mylv
```

You may be asked to confirm the operation.

Verify:

```bash
sudo lvs
```

---

## Step 4: Remove the Volume Group

Once the logical volume has been removed:

```bash
sudo vgremove myvg
```

Verify:

```bash
sudo vgs
```

The `myvg` volume group should no longer appear.

---

## Step 5: Remove the Physical Volume

Finally, remove the LVM metadata from the test disk:

```bash
sudo pvremove /dev/sdb
```

Verify:

```bash
sudo pvs
```

The test disk should no longer appear as an LVM physical volume.

---

# 🔄 Final Storage Verification

Run:

```bash
lsblk
```

Then:

```bash
sudo pvs
sudo vgs
sudo lvs
```

The test LVM configuration should no longer be present.

If the mount point is no longer needed, remove it:

```bash
sudo rmdir /mnt/mylv
```

---

# 🧰 Useful LVM Commands

| Command     | Purpose                                    |
| ----------- | ------------------------------------------ |
| `pvs`       | Display physical volumes                   |
| `pvdisplay` | Detailed physical-volume information       |
| `pvcreate`  | Create a physical volume                   |
| `pvremove`  | Remove LVM metadata from a physical volume |
| `vgs`       | Display volume groups                      |
| `vgdisplay` | Detailed volume-group information          |
| `vgcreate`  | Create a volume group                      |
| `vgremove`  | Remove a volume group                      |
| `lvs`       | Display logical volumes                    |
| `lvdisplay` | Detailed logical-volume information        |
| `lvcreate`  | Create a logical volume                    |
| `lvremove`  | Remove a logical volume                    |
| `lsblk`     | Display block devices                      |
| `blkid`     | Display filesystem and UUID information    |

---

# ⚠️ Troubleshooting

## Problem: `pvcreate` reports that the device is in use

Check the device:

```bash
lsblk
```

Check whether it has mounted filesystems:

```bash
findmnt
```

Do not proceed until you are certain the disk is safe to use.

---

## Problem: `lvremove` says the logical volume is still in use

Check whether it is mounted:

```bash
findmnt
```

If it is mounted:

```bash
sudo umount /mnt/mylv
```

Then retry:

```bash
sudo lvremove /dev/myvg/mylv
```

---

## Problem: `umount` says the target is busy

Make sure your current shell is not inside the mount point:

```bash
cd ~
```

Then check for processes using it:

```bash
sudo fuser -vm /mnt/mylv
```

You can also use:

```bash
sudo lsof +D /mnt/mylv
```

---

## Problem: `vgremove` cannot remove the volume group

Check for remaining logical volumes:

```bash
sudo lvs
```

Remove any remaining test LVs before removing the volume group.

---

## Problem: `pvremove` cannot remove the physical volume

Check:

```bash
sudo pvs
sudo vgs
sudo lvs
```

The physical volume should no longer belong to a volume group before running:

```bash
sudo pvremove /dev/sdb
```

---

# 🔐 Safety and Best Practices

LVM provides powerful storage-management capabilities, but incorrect commands can result in data loss.

Always:

* Verify the correct disk with `lsblk`.
* Confirm the disk is not being used by the operating system.
* Use a dedicated test disk for practice.
* Never run `pvcreate` on a production disk without proper planning.
* Never run `mkfs` on an unknown device.
* Back up important data before storage changes.
* Unmount filesystems before removing their logical volumes.
* Remove LVM components in the correct order.
* Double-check destructive commands before confirming them.

---

# 💼 Practical Case Study

### Scenario: Flexible Storage for a Linux Server

A Linux administrator has an additional 5 GB disk:

```text
/dev/sdb
```

Instead of creating traditional fixed partitions, the administrator uses LVM.

The disk is initialized:

```bash
sudo pvcreate /dev/sdb
```

A volume group is created:

```bash
sudo vgcreate myvg /dev/sdb
```

A 1 GB logical volume is created:

```bash
sudo lvcreate -L 1G -n mylv myvg
```

The LV is formatted:

```bash
sudo mkfs.ext4 /dev/myvg/mylv
```

A mount point is created:

```bash
sudo mkdir -p /mnt/mylv
```

The logical volume is mounted:

```bash
sudo mount /dev/myvg/mylv /mnt/mylv
```

The administrator can now use:

```text
/mnt/mylv
```

as normal storage.

One of the major advantages of LVM is that the logical volume can later be extended when additional free space is available in the volume group.

For example, an administrator could investigate available space with:

```bash
sudo vgs
```

and later extend an LV using appropriate LVM and filesystem-resize commands.

---

# 🧠 LVM Architecture Summary

The complete structure used in this lab is:

```text
/dev/sdb
   │
   ▼
Physical Volume
   │
   ▼
myvg
Volume Group
   │
   ▼
mylv
Logical Volume
   │
   ▼
ext4
Filesystem
   │
   ▼
/mnt/mylv
Mount Point
```

Remember the three main LVM layers:

```text
PV → VG → LV
```

**Physical Volume → Volume Group → Logical Volume**

---

# 📝 Lab Verification Checklist

* [ ] Checked existing logical volumes with `lvdisplay` or `lvs`.
* [ ] Checked existing volume groups with `vgs`.
* [ ] Checked existing physical volumes with `pvs`.
* [ ] Identified a safe test disk.
* [ ] Created a physical volume.
* [ ] Verified the physical volume.
* [ ] Created the `myvg` volume group.
* [ ] Verified the volume group.
* [ ] Created the `mylv` logical volume.
* [ ] Verified the logical volume.
* [ ] Formatted the LV with `ext4`.
* [ ] Created `/mnt/mylv`.
* [ ] Mounted the logical volume.
* [ ] Verified the mounted filesystem.
* [ ] Created and verified a test file.
* [ ] Unmounted the logical volume.
* [ ] Removed the logical volume.
* [ ] Removed the volume group.
* [ ] Removed the physical volume.
* [ ] Performed final LVM verification.

---

# 🔬 Further Study

After completing the basic LVM lab, explore these advanced topics:

### Extend a Logical Volume

Learn how to increase the size of an LV:

```bash
sudo lvextend
```

### Resize a Filesystem

For an `ext4` filesystem, investigate:

```bash
sudo resize2fs
```

### LVM Snapshots

Explore snapshots for backup and testing:

```bash
sudo lvcreate --snapshot
```

### Reduce Logical Volumes

Study the correct procedure for reducing an LV and filesystem. This operation requires additional care because incorrect steps can cause data loss.

### LVM Thin Provisioning

Explore thin pools for more flexible storage allocation.

### LVM RAID

Study LVM-based RAID configurations for redundancy and performance.

---

# 🏁 Conclusion

In this lab, you learned the fundamental building blocks of **Linux Logical Volume Manager (LVM)**.

You created a complete LVM storage stack:

```text
Physical Volume → Volume Group → Logical Volume → Filesystem → Mount Point
```

You also practiced inspecting LVM components, formatting and mounting a logical volume, creating test data, and safely removing the LVM configuration.

These skills provide an important foundation for **Linux system administration, server management, DevOps, cloud infrastructure, and enterprise storage management**.

---

## 📂 Lab Structure

```text
Lab-44-Basic-LVM-Concepts/
└── README.md
```

---

**Lab Status:** ✅ Completed

**Skills Practiced:** `pvcreate` · `pvs` · `pvdisplay` · `vgcreate` · `vgs` · `vgdisplay` · `lvcreate` · `lvs` · `lvdisplay` · `mkfs.ext4` · `mount` · `umount` · `lvremove` · `vgremove` · `pvremove` · `lsblk`
