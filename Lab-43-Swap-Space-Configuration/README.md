# Lab 43: Swap Space Configuration

## 📌 Lab Overview

This lab focuses on **swap space configuration in Linux**. Swap provides disk-based virtual memory that the Linux kernel can use when physical RAM becomes heavily utilized.

You will learn how to inspect existing swap space, create a swap file, secure it with appropriate permissions, initialize it, enable it, verify its operation, and configure it to activate automatically after a reboot.

---

## 🎯 Objectives

By completing this lab, you will:

* Understand what swap space is and why Linux uses it.
* Check the current swap configuration and usage.
* Create a swap file using `fallocate`.
* Set secure permissions on a swap file.
* Initialize a file as swap space using `mkswap`.
* Enable swap using `swapon`.
* Verify active swap space.
* Configure persistent swap using `/etc/fstab`.
* Test the configuration safely.

---

## 🛠️ Prerequisites

Before starting this lab, you should have:

* Basic Linux command-line knowledge.
* Access to a Linux system.
* `sudo` or root privileges.
* Basic knowledge of filesystems and disk storage.
* Familiarity with `nano` or another text editor.
* Sufficient free disk space for the swap file.

> ⚠️ **Important:** Creating a swap file consumes disk space. Check available disk space before starting.

---

# 📚 Key Concepts

## What Is Swap Space?

**Swap space** is disk storage that Linux can use as additional virtual memory.

When physical RAM becomes highly utilized, Linux can move less frequently used memory pages from RAM to swap space.

```text
              Linux System
                   │
          ┌────────┴────────┐
          │                 │
        RAM              Swap Space
     (Physical)            (Disk)
          │                 │
     Fast access       Slower access
```

Swap is **not a replacement for RAM**. Because disk storage is significantly slower than physical memory, excessive swapping can negatively affect system performance.

---

## Swap File vs Swap Partition

Linux can use:

* A dedicated swap partition.
* A swap file.

This lab uses a **swap file** because it is flexible and easier to create or remove without repartitioning a disk.

---

# 🔍 Task 1: Check Current Swap Usage

## Step 1: Open the Terminal

Connect to your Linux system through SSH or open a local terminal.

---

## Step 2: Check Active Swap Areas

Run:

```bash
swapon --show
```

If swap is configured, you may see output similar to:

```text
NAME      TYPE SIZE USED PRIO
/swapfile file   1G   0B   -2
```

If no output is displayed, there may currently be no active swap area.

---

## Step 3: Check Memory and Swap Usage

Run:

```bash
free -h
```

Example:

```text
               total        used        free      shared  buff/cache   available
Mem:           7.7Gi       1.2Gi       5.5Gi       10Mi       1.0Gi       6.3Gi
Swap:          1.0Gi          0B       1.0Gi
```

The `-h` option displays values in a human-readable format.

---

## Step 4: Check `/proc/swaps`

Linux also exposes active swap information through:

```bash
cat /proc/swaps
```

Example:

```text
Filename                Type        Size    Used    Priority
/swapfile               file        1048572 0       -2
```

---

# 💾 Task 2: Check Available Disk Space

Before creating the swap file, verify that sufficient disk space is available.

Run:

```bash
df -h /
```

Example:

```text
Filesystem      Size  Used Avail Use% Mounted on
/dev/root        20G  8.0G   12G  40% /
```

A 1 GB swap file requires at least 1 GB of available disk space, plus some additional space for normal system operation.

---

# 📝 Task 3: Create the Swap File

## Step 1: Choose the Swap File Location

For this lab, use:

```text
/swapfile
```

The example swap file size is:

```text
1 GB
```

---

## Step 2: Create the Swap File

Use `fallocate`:

```bash
sudo fallocate -l 1G /swapfile
```

The `-l` option specifies the size of the file.

---

## Step 3: Verify the File

Run:

```bash
ls -lh /swapfile
```

Expected output:

```text
-rw-r--r-- 1 root root 1.0G ... /swapfile
```

---

# 🔐 Task 4: Secure the Swap File

A swap file can contain memory pages that may include sensitive information. Therefore, it should not be readable or writable by ordinary users.

Set its permissions to `600`:

```bash
sudo chmod 600 /swapfile
```

Verify:

```bash
ls -lh /swapfile
```

Expected permissions:

```text
-rw------- 1 root root 1.0G ... /swapfile
```

This means:

* Owner: read/write
* Group: no access
* Others: no access

---

# ⚙️ Task 5: Initialize the Swap File

Convert the file into a swap area using:

```bash
sudo mkswap /swapfile
```

Example output:

```text
Setting up swapspace version 1, size = 1024 MiB
no label, UUID=xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx
```

The exact UUID will vary between systems.

---

## Verify the Swap Signature

You can check the file with:

```bash
sudo file /swapfile
```

It should identify the file as a swap area.

---

# 🚀 Task 6: Enable the Swap File

Activate the swap file:

```bash
sudo swapon /swapfile
```

No output normally indicates that the command succeeded.

---

# ✅ Task 7: Verify Swap Activation

Run:

```bash
swapon --show
```

Expected output:

```text
NAME      TYPE SIZE USED PRIO
/swapfile file   1G   0B   -2
```

You can also verify it with:

```bash
free -h
```

The `Swap` row should now show the configured swap size.

---

## Verify Using `findmnt`

Run:

```bash
findmnt -T /swapfile
```

Note that swap is not a normal mounted filesystem; `swapon --show` and `free -h` are the primary verification commands.

---

# 🔄 Task 8: Make Swap Permanent

By default, activating swap with `swapon` does not necessarily make it persistent across reboots.

To automatically enable it during boot, configure `/etc/fstab`.

---

## Step 1: Back Up `/etc/fstab`

Before editing an important system configuration file, create a backup:

```bash
sudo cp /etc/fstab /etc/fstab.backup
```

Verify:

```bash
ls -l /etc/fstab.backup
```

---

## Step 2: Edit `/etc/fstab`

Open the file:

```bash
sudo nano /etc/fstab
```

Add this line:

```text
/swapfile none swap sw 0 0
```

Save the file and exit.

For Nano:

```text
Ctrl + O
Enter
Ctrl + X
```

---

# 🧪 Task 9: Test the `/etc/fstab` Configuration

It is important to test the configuration before rebooting.

First, disable the currently active swap:

```bash
sudo swapoff /swapfile
```

Verify:

```bash
swapon --show
```

Then activate swap using the `/etc/fstab` configuration:

```bash
sudo swapon -a
```

Verify again:

```bash
swapon --show
```

You should see:

```text
NAME      TYPE SIZE USED PRIO
/swapfile file   1G   0B   -2
```

This confirms that the `/etc/fstab` entry is working.

---

# 🔄 Task 10: Optional Reboot Test

If this is a lab or test machine, you can verify persistence after reboot.

Before rebooting:

```bash
swapon --show
```

Then:

```bash
sudo reboot
```

After reconnecting, run:

```bash
swapon --show
```

And:

```bash
free -h
```

The swap file should automatically be active.

> ⚠️ Do not reboot a production server without confirming that a reboot is safe.

---

# 🔧 Useful Swap Commands

### Display active swap

```bash
swapon --show
```

### Display memory and swap

```bash
free -h
```

### Display swap information

```bash
cat /proc/swaps
```

### Enable a swap file

```bash
sudo swapon /swapfile
```

### Disable a swap file

```bash
sudo swapoff /swapfile
```

### Enable all swap entries from `/etc/fstab`

```bash
sudo swapon -a
```

### Initialize a swap file

```bash
sudo mkswap /swapfile
```

---

# 🛠️ Troubleshooting

## Problem: `swapon: /swapfile: insecure permissions`

Check the permissions:

```bash
ls -l /swapfile
```

Set them correctly:

```bash
sudo chmod 600 /swapfile
```

Then retry:

```bash
sudo swapon /swapfile
```

---

## Problem: `swapon failed: Invalid argument`

Check whether the file was initialized as swap:

```bash
sudo mkswap /swapfile
```

Then:

```bash
sudo swapon /swapfile
```

---

## Problem: `/swapfile already exists`

If an existing swap file is present, do not overwrite it blindly.

Check:

```bash
ls -lh /swapfile
```

Then:

```bash
swapon --show
```

If it is already active, no new swap file needs to be created.

---

## Problem: `No space left on device`

Check available disk space:

```bash
df -h
```

Remove unnecessary files or choose an appropriate swap size.

---

## Problem: Duplicate `/etc/fstab` Entry

Check for existing swap entries:

```bash
grep -n swap /etc/fstab
```

Avoid adding duplicate entries for the same swap file.

---

# 🔐 Security Considerations

Swap can contain data that was previously present in RAM. This means sensitive information may potentially be written to disk.

For systems handling sensitive information, administrators should consider appropriate swap-security controls, including encrypted swap where required.

For this basic lab, the important security practice is:

```bash
sudo chmod 600 /swapfile
```

This prevents ordinary users from directly reading the swap file.

---

# 📊 Verification Summary

| Check                 | Command            | Purpose                 |
| --------------------- | ------------------ | ----------------------- |
| Current swap          | `swapon --show`    | Display active swap     |
| Memory usage          | `free -h`          | View RAM and swap       |
| Disk space            | `df -h /`          | Check available storage |
| Swap information      | `cat /proc/swaps`  | Inspect active swap     |
| File permissions      | `ls -lh /swapfile` | Verify security         |
| Initialize swap       | `mkswap`           | Create swap signature   |
| Enable swap           | `swapon`           | Activate swap           |
| Disable swap          | `swapoff`          | Deactivate swap         |
| Persistent activation | `/etc/fstab`       | Enable swap at boot     |

---

# 📝 Lab Verification Checklist

* [ ] Checked the current swap configuration.
* [ ] Used `free -h` to inspect memory and swap.
* [ ] Checked available disk space.
* [ ] Created `/swapfile`.
* [ ] Set permissions to `600`.
* [ ] Initialized the file with `mkswap`.
* [ ] Enabled the swap file.
* [ ] Verified the swap file with `swapon --show`.
* [ ] Verified swap usage with `free -h`.
* [ ] Backed up `/etc/fstab`.
* [ ] Added the swap file to `/etc/fstab`.
* [ ] Tested the `/etc/fstab` configuration using `swapon -a`.
* [ ] Optionally verified persistence after reboot.

---

# 💼 Practical Case Study

### Scenario: Low-Memory Linux Server

Imagine a Linux server with limited physical RAM that occasionally experiences high memory utilization.

The administrator first checks memory and swap:

```bash
free -h
```

They discover that no swap is currently configured.

The administrator checks disk space:

```bash
df -h /
```

After confirming sufficient free space, they create a 1 GB swap file:

```bash
sudo fallocate -l 1G /swapfile
```

They secure it:

```bash
sudo chmod 600 /swapfile
```

Initialize it:

```bash
sudo mkswap /swapfile
```

Enable it:

```bash
sudo swapon /swapfile
```

Finally, they verify:

```bash
swapon --show
```

and:

```bash
free -h
```

To ensure swap remains available after a reboot, they add:

```text
/swapfile none swap sw 0 0
```

to `/etc/fstab`.

This gives the Linux system additional virtual-memory capacity during periods of high memory pressure.

---

# 🧠 Key Takeaways

* Swap provides disk-based virtual memory.
* Swap is slower than physical RAM and should not be treated as a replacement for RAM.
* `swapon --show` displays active swap areas.
* `free -h` provides a quick overview of RAM and swap usage.
* `fallocate` can create a swap file.
* `chmod 600` protects the swap file from ordinary users.
* `mkswap` initializes a file as swap space.
* `swapon` activates swap.
* `swapoff` deactivates swap.
* `/etc/fstab` can be used to make swap persistent across reboots.
* Always verify available disk space before creating a swap file.

---

# 🏁 Conclusion

In this lab, you learned how to manage **swap space using a swap file** on Linux.

You inspected the existing swap configuration, created a 1 GB swap file, secured its permissions, initialized it with `mkswap`, enabled it with `swapon`, verified its operation, and configured `/etc/fstab` for persistent activation.

Swap management is an important Linux administration skill and is particularly useful when managing servers with limited physical memory.

---

## 📂 Lab Structure

```text
Lab-43-Swap-Space-Configuration/
└── README.md
```

---

**Lab Status:** ✅ Completed

**Skills Practiced:** `swapon` · `swapoff` · `mkswap` · `fallocate` · `chmod` · `free` · `df` · `/etc/fstab`
