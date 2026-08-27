# Lab 45: Introduction to `/etc/fstab`

## 📌 Lab Overview

This lab introduces the **`/etc/fstab`** configuration file, one of the most important files for Linux filesystem management.

The `/etc/fstab` file defines filesystems that Linux should mount automatically and specifies **where and how those filesystems should be mounted**.

In this lab, you will inspect the existing `/etc/fstab` configuration, understand its six-field structure, identify a storage device, create a mount point, safely add a filesystem entry, and validate the configuration using `mount -a`.

---

## 🎯 Lab Objectives

By completing this lab, you will:

* Understand the purpose of `/etc/fstab`.
* Learn how Linux uses `/etc/fstab` during boot.
* View and interpret existing filesystem entries.
* Understand the six fields used in an `/etc/fstab` entry.
* Identify disks, partitions, filesystems, and UUIDs.
* Create a mount point for a test filesystem.
* Back up `/etc/fstab` before making changes.
* Add a filesystem entry to `/etc/fstab`.
* Test the configuration without rebooting.
* Verify that the filesystem is mounted correctly.
* Understand common `/etc/fstab` mistakes and recovery procedures.

---

## 🛠️ Prerequisites

Before starting this lab, you should have:

* Basic Linux command-line knowledge.
* Understanding of filesystems and mount points.
* Access to a Linux system.
* `sudo` or root privileges.
* Familiarity with `nano` or another text editor.
* Preferably, an additional test partition or removable storage device.

> ⚠️ **IMPORTANT:** `/etc/fstab` is a critical system configuration file. An incorrect entry can cause boot problems or prevent filesystems from mounting correctly. Always create a backup and test changes before rebooting.

---

# 📚 What Is `/etc/fstab`?

`/etc/fstab` stands for **filesystem table**.

It contains information about filesystems that Linux can use to determine:

* What device or filesystem should be mounted.
* Where it should be mounted.
* Which filesystem type it uses.
* Which mount options should be applied.
* Whether it should be included in filesystem backup operations.
* Whether filesystem checks should be performed during boot.

A simplified example:

```text id="sm6uw4"
/dev/sdb1  /media/usb  ext4  defaults  0  2
```

This tells Linux to mount `/dev/sdb1` at `/media/usb` using the `ext4` filesystem and the specified options.

---

# 🧩 The Six Fields of `/etc/fstab`

Each filesystem entry normally contains six fields:

```text id="k3t2c7"
<device> <mount-point> <filesystem-type> <options> <dump> <fsck>
```

Example:

```text id="a8qk8a"
/dev/sdb1 /media/usb ext4 defaults 0 2
```

---

## 1. Device / Filesystem Identifier

Example:

```text id="3b6o4p"
/dev/sdb1
```

This identifies the filesystem to be mounted.

Instead of a device name, a UUID is often preferred:

```text id="h0q9p7"
UUID=xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx
```

Using UUIDs is generally more reliable because device names can change depending on how storage devices are detected.

---

## 2. Mount Point

Example:

```text id="w5c1tm"
/media/usb
```

This is the directory where the filesystem becomes accessible.

Other common mount points include:

```text id="a7q1se"
/home
/var
/mnt/data
```

---

## 3. Filesystem Type

Example:

```text id="gq7d2s"
ext4
```

Common filesystem types include:

* `ext4`
* `xfs`
* `btrfs`
* `vfat`
* `ntfs`
* `swap`

The correct filesystem type should always be verified before adding an entry.

---

## 4. Mount Options

Example:

```text id="9p1yhz"
defaults
```

Common options include:

* `defaults`
* `ro` — read-only
* `rw` — read/write
* `noexec` — prevent execution of binaries
* `nosuid` — ignore SUID/SGID bits
* `nodev` — prevent device files

For a basic lab, `defaults` is sufficient.

---

## 5. Dump Field

Example:

```text id="r2m5gs"
0
```

The fifth field is related to the legacy `dump` backup utility.

For most modern systems, this is commonly set to:

```text id="9eq4jv"
0
```

---

## 6. Filesystem Check (`fsck`) Order

Example:

```text id="3y1zll"
2
```

This field determines the order in which filesystem checks are performed.

Common values:

```text id="0s3k9d"
0 = Do not check
1 = Check first
2 = Check after filesystems marked 1
```

The root filesystem is commonly associated with `1`, while other filesystems may use `2`.

---

# 🔍 Task 1: Review Existing `/etc/fstab`

## Step 1: View the File

You can open `/etc/fstab` with:

```bash id="k1o2x8"
sudo nano /etc/fstab
```

For a safer read-only view, use:

```bash id="4pk0ba"
cat /etc/fstab
```

Or:

```bash id="0wq5ly"
less /etc/fstab
```

---

## Step 2: Identify Existing Entries

A typical `/etc/fstab` may contain entries similar to:

```text id="9a8wpl"
# /etc/fstab: static file system information.

UUID=xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx /     ext4  defaults  0 1
UUID=xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx none  swap  sw        0 0
```

Your actual configuration will vary depending on the Linux distribution and system setup.

---

# 🔎 Task 2: Understand Existing Mounts

Compare `/etc/fstab` with the currently mounted filesystems.

Run:

```bash id="l4p7ko"
findmnt
```

You can also run:

```bash id="g7k0z3"
df -hT
```

This allows you to compare:

```text id="x2o3h4"
/etc/fstab
     ↓
Configured filesystems

findmnt / df
     ↓
Currently mounted filesystems
```

---

# 💽 Task 3: Identify the Test Storage Device

If you have an additional disk, partition, or removable storage device, identify it with:

```bash id="q6k4eu"
lsblk
```

For detailed filesystem information:

```bash id="9gq3jv"
lsblk -f
```

A useful customized view is:

```bash id="i8p2dr"
lsblk -o NAME,SIZE,TYPE,FSTYPE,UUID,MOUNTPOINTS
```

Example:

```text id="h4qz8w"
NAME        SIZE TYPE FSTYPE UUID                                 MOUNTPOINTS
nvme0n1      20G disk
├─nvme0n1p1  19G part ext4   xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx /
└─nvme0n1p2   1G part swap   xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx [SWAP]
nvme1n1       5G disk
└─nvme1n1p1   5G part ext4   xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx
```

In this example, `/dev/nvme1n1p1` could be used as the test partition.

> ⚠️ **Do not assume that `/dev/sdb1` or `/dev/nvme1n1p1` is your device.** Always use `lsblk -f` to identify the correct partition.

---

# 🆔 Task 4: Identify the Filesystem UUID

UUIDs provide a persistent identifier for a filesystem.

Run:

```bash id="i7t0sl"
sudo blkid
```

Or for a specific partition:

```bash id="0un6j7"
sudo blkid /dev/sdb1
```

Example:

```text id="w7j3h0"
/dev/sdb1: UUID="1234-5678-ABCD" TYPE="ext4"
```

The UUID can then be used in `/etc/fstab`:

```text id="1a3p9s"
UUID=1234-5678-ABCD /media/usb ext4 defaults 0 2
```

---

# 💾 Task 5: Back Up `/etc/fstab`

Before making any modifications, create a backup.

Run:

```bash id="z4y0e2"
sudo cp /etc/fstab /etc/fstab.bak
```

Verify the backup:

```bash id="m5t6xc"
ls -l /etc/fstab /etc/fstab.bak
```

You now have a recovery copy of the original configuration.

---

# 📁 Task 6: Create a Mount Point

Create a directory for the test filesystem:

```bash id="q0d5fn"
sudo mkdir -p /media/usb
```

Verify:

```bash id="3i5u1c"
ls -ld /media/usb
```

---

# 📝 Task 7: Prepare the `/etc/fstab` Entry

For example, suppose:

```text id="6jq1l8"
/dev/sdb1
```

is your test partition and it uses `ext4`.

A basic entry would be:

```text id="9j2w5m"
/dev/sdb1 /media/usb ext4 defaults 0 2
```

However, using the UUID is generally preferable:

```text id="m7c5ri"
UUID=YOUR-UUID-HERE /media/usb ext4 defaults 0 2
```

Replace `YOUR-UUID-HERE` with the actual UUID obtained from `lsblk -f` or `blkid`.

---

# ✏️ Task 8: Edit `/etc/fstab`

Open the file:

```bash id="3h5q6p"
sudo nano /etc/fstab
```

Add the appropriate entry at the bottom.

Example:

```text id="v6k9q1"
UUID=xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx /media/usb ext4 defaults 0 2
```

Save the file:

```text id="q3c8fj"
Ctrl + O
Enter
Ctrl + X
```

---

# 🧪 Task 9: Test the `/etc/fstab` Configuration

**Do not reboot immediately after editing `/etc/fstab`.**

First test the configuration with:

```bash id="2j8v9s"
sudo mount -a
```

`mount -a` attempts to mount filesystems listed in `/etc/fstab` that are not already mounted and are not excluded by their options.

### Successful Test

If the command produces no error, the configuration is likely valid.

> ⚠️ A silent `mount -a` does not replace verification. Always check that the expected filesystem was actually mounted.

---

# 🔎 Task 10: Verify the Mount

Check the mount point:

```bash id="x0d6ms"
findmnt /media/usb
```

You can also run:

```bash id="3j1t9a"
df -hT /media/usb
```

Example:

```text id="j7f2lm"
Filesystem     Type  Size  Used Avail Use% Mounted on
/dev/sdb1      ext4  4.9G   24K  4.6G   1% /media/usb
```

You can also verify with:

```bash id="a5z6up"
mountpoint /media/usb
```

Expected output:

```text id="n3y5co"
/media/usb is a mountpoint
```

---

# 🧪 Task 11: Test Access to the Filesystem

Change into the mount point:

```bash id="r4n8we"
cd /media/usb
```

List its contents:

```bash id="j0q7vp"
ls -la
```

If the filesystem is writable, create a temporary test file:

```bash id="0m8y9f"
sudo touch /media/usb/fstab-test.txt
```

Verify:

```bash id="9v6x2d"
ls -l /media/usb/fstab-test.txt
```

After testing, remove the file:

```bash id="z8f4u3"
sudo rm /media/usb/fstab-test.txt
```

---

# 🔄 Task 12: Test Unmount and Remount

To verify that the `/etc/fstab` entry can recreate the mount:

First leave the directory:

```bash id="w2y6px"
cd ~
```

Unmount:

```bash id="6p0z4s"
sudo umount /media/usb
```

Verify:

```bash id="2w8j7q"
findmnt /media/usb
```

Then remount using `/etc/fstab`:

```bash id="r5y9ak"
sudo mount /media/usb
```

Verify again:

```bash id="f3q7ds"
findmnt /media/usb
```

This confirms that the mount point can be restored using the configuration stored in `/etc/fstab`.

---

# ⚠️ Troubleshooting

## Error: `mount point does not exist`

Example:

```text id="v8y3qf"
mount: /media/usb: mount point does not exist
```

Create the mount point:

```bash id="c4h7r2"
sudo mkdir -p /media/usb
```

Then test:

```bash id="j5m2nk"
sudo mount -a
```

---

## Error: `wrong fs type`

Check the filesystem type:

```bash id="t8f1pw"
lsblk -f
```

or:

```bash id="n2v5sl"
sudo blkid
```

Make sure the filesystem type in `/etc/fstab` matches the actual filesystem.

---

## Error: `special device does not exist`

This can happen if the device name or UUID is incorrect.

Check:

```bash id="d4s6kj"
lsblk -f
```

Then compare the actual UUID with the UUID in `/etc/fstab`.

---

## Error: `target is busy`

If unmounting fails:

```bash id="k6w3se"
sudo umount /media/usb
```

Check which processes are using the mount:

```bash id="b2f7nx"
sudo fuser -vm /media/usb
```

Also make sure your current terminal is not inside the mount point:

```bash id="m0x9qk"
cd ~
```

---

# 🚨 What If `/etc/fstab` Contains an Error?

If you accidentally add an invalid entry, **do not immediately reboot**.

First restore the backup if necessary:

```bash id="e5c1sv"
sudo cp /etc/fstab.bak /etc/fstab
```

Then test:

```bash id="g7n4xz"
sudo mount -a
```

If the system is already experiencing boot problems because of an `/etc/fstab` entry, recovery procedures may be required depending on the Linux distribution and boot environment.

This is why testing with:

```bash id="z9h2kc"
sudo mount -a
```

before rebooting is an important administration practice.

---

# 🔐 Best Practices for `/etc/fstab`

When editing `/etc/fstab`:

* Always create a backup first.
* Prefer UUIDs over unstable device names when appropriate.
* Verify filesystem types with `lsblk -f`.
* Ensure mount-point directories exist.
* Avoid unnecessary changes to existing system entries.
* Test changes with `sudo mount -a`.
* Verify the resulting mount with `findmnt`.
* Do not reboot until the configuration has been tested.
* Keep production configuration changes documented.

---

# 📊 Useful Commands

| Command                        | Purpose                                    |
| ------------------------------ | ------------------------------------------ |
| `cat /etc/fstab`               | Display the configuration                  |
| `sudo nano /etc/fstab`         | Edit the configuration                     |
| `lsblk`                        | Display block devices                      |
| `lsblk -f`                     | Display filesystem information             |
| `blkid`                        | Display UUIDs and filesystem types         |
| `findmnt`                      | Display mounted filesystems                |
| `df -hT`                       | Display filesystem usage and type          |
| `mount -a`                     | Mount applicable `/etc/fstab` entries      |
| `mount /media/usb`             | Mount using the configured fstab entry     |
| `umount /media/usb`            | Unmount a filesystem                       |
| `mountpoint`                   | Check whether a directory is a mount point |
| `cp /etc/fstab /etc/fstab.bak` | Back up `/etc/fstab`                       |

---

# 🧠 `/etc/fstab` Entry Example

A complete example:

```text id="z4l6qp"
UUID=12345678-abcd-1234-abcd-123456789abc /media/usb ext4 defaults 0 2
```

Breakdown:

```text id="v5n1rs"
UUID=12345678-abcd-1234-abcd-123456789abc
        ↓
Filesystem identifier

/media/usb
        ↓
Mount point

ext4
        ↓
Filesystem type

defaults
        ↓
Mount options

0
        ↓
Dump setting

2
        ↓
Filesystem check order
```

---

# 💼 Practical Case Study

### Scenario: Automatically Mounting a Data Partition

A Linux administrator has an additional data partition:

```text id="n7k2bc"
/dev/sdb1
```

The administrator first checks the filesystem:

```bash id="5q1s9k"
lsblk -f
```

Suppose it reports:

```text id="y3p6vm"
NAME   FSTYPE UUID
sdb1   ext4   12345678-abcd-1234-abcd-123456789abc
```

The administrator creates a mount point:

```bash id="q8w4za"
sudo mkdir -p /media/usb
```

They back up `/etc/fstab`:

```bash id="0r6j1x"
sudo cp /etc/fstab /etc/fstab.bak
```

Then they add:

```text id="t7k5ps"
UUID=12345678-abcd-1234-abcd-123456789abc /media/usb ext4 defaults 0 2
```

Instead of rebooting, they test the configuration:

```bash id="h4n8cz"
sudo mount -a
```

Then verify:

```bash id="x6q2dw"
findmnt /media/usb
```

and:

```bash id="m8r3vy"
df -hT /media/usb
```

The filesystem is now configured to be automatically mounted according to the `/etc/fstab` entry.

---

# 📝 Lab Verification Checklist

* [ ] Viewed `/etc/fstab`.
* [ ] Identified existing filesystem entries.
* [ ] Understood the six fields in an `/etc/fstab` entry.
* [ ] Checked currently mounted filesystems.
* [ ] Identified a safe test partition.
* [ ] Used `lsblk -f` to determine filesystem information.
* [ ] Retrieved the filesystem UUID.
* [ ] Created a backup of `/etc/fstab`.
* [ ] Created the required mount point.
* [ ] Added the filesystem entry.
* [ ] Tested the configuration using `mount -a`.
* [ ] Verified the mount using `findmnt`.
* [ ] Verified filesystem usage using `df -hT`.
* [ ] Tested unmounting and remounting.
* [ ] Confirmed the final configuration.

---

# 🔬 Further Study

After completing this introductory lab, explore:

### UUID vs Device Names

Understand why:

```text id="6j1v5z"
/dev/sdb1
```

may be less reliable than:

```text id="h3q8mx"
UUID=xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx
```

for persistent configuration.

### Mount Options

Explore options such as:

```text id="n9f2q4"
ro
rw
noexec
nosuid
nodev
noauto
nofail
```

### `nofail`

Investigate how `nofail` can be useful for optional storage devices so that the absence of a device does not unnecessarily prevent normal system startup.

### Automounting

Explore systemd automount features and how they interact with `/etc/fstab`.

### Filesystem Security

Study how mount options can improve security by restricting executable files, device files, and SUID behavior on specific filesystems.

---

# 🏁 Conclusion

In this lab, you learned how **`/etc/fstab`** controls persistent filesystem mounting in Linux.

You inspected existing entries, learned the six-field structure, identified a test filesystem and its UUID, created a mount point, backed up the configuration, added a new entry, and tested it safely using `mount -a`.

The most important administration workflow is:

```text id="x0v4kc"
Identify Device
      ↓
Check Filesystem + UUID
      ↓
Create Mount Point
      ↓
Back Up /etc/fstab
      ↓
Add Entry
      ↓
Run: mount -a
      ↓
Verify with findmnt / df
      ↓
Only Then Consider Rebooting
```

Understanding `/etc/fstab` is an essential Linux administration skill because it enables reliable and persistent filesystem configuration across system reboots.

---

## 📂 Lab Structure

```text id="n8j5yt"
Lab-45-Introduction-to-fstab/
└── README.md
```

---

**Lab Status:** ✅ Completed

**Skills Practiced:** `fstab` · `lsblk` · `lsblk -f` · `blkid` · `mount` · `mount -a` · `umount` · `findmnt` · `df` · `nano` · `UUID`
