# ⚙️ Lab 38: Systemd Services Overview

> **Linux System Administration | Service Management | systemctl**

---

## 📌 Lab Overview

**systemd** is the modern initialization and service-management system used by many Linux distributions. It is responsible for starting system components during boot and managing services throughout the system's runtime.

In this lab, you will work with **`systemctl`**, the primary command-line utility used to inspect and manage systemd services.

You will learn how to:

* List running services.
* Check service status.
* Start and stop services.
* Enable services at boot.
* Disable services from starting automatically.
* Understand the difference between a service's current state and its boot-time configuration.

---

## 🎯 Objectives

By completing this lab, you will learn how to:

* Understand the role of **systemd** in Linux.
* Identify active systemd services.
* Use `systemctl` to inspect service status.
* Start and stop system services.
* Enable services to start automatically at boot.
* Disable services from starting automatically.
* Develop practical Linux server administration skills.

---

## 🛠️ Prerequisites

* Basic understanding of Linux.
* Access to a Linux system using systemd.
* Terminal or SSH access.
* A user account with `sudo` privileges.
* Basic familiarity with Linux commands.

---

# 📚 1. Understanding systemd

### What is systemd?

**systemd** is a system and service manager for Linux.

It is responsible for managing many important parts of a Linux system, including:

* System startup
* Background services
* Service dependencies
* Mount points
* Timers
* Logging integration
* System states and targets

Services managed by systemd are commonly represented as **`.service` units**.

For example:

```text
ssh.service
cron.service
systemd-journald.service
```

---

# 🔧 2. Understanding `systemctl`

`systemctl` is the primary command used to communicate with systemd.

### Basic Syntax

```bash
systemctl [command] [unit]
```

Examples:

```bash
systemctl status ssh
```

```bash
sudo systemctl restart ssh
```

```bash
sudo systemctl enable ssh
```

---

# 🧪 Task 1: List Running Services

## Step 1 — List Active Services

Run:

```bash
systemctl list-units --type=service
```

This displays currently loaded service units.

### Example Output

```text
UNIT                         LOAD   ACTIVE SUB     DESCRIPTION
cron.service                 loaded active running Regular background program processing daemon
ssh.service                  loaded active running OpenBSD Secure Shell server
systemd-journald.service     loaded active running Journal Service
```

---

## 🔎 Understanding the Output

The most important columns are:

| Column        | Meaning                                                |
| ------------- | ------------------------------------------------------ |
| `UNIT`        | Name of the systemd unit                               |
| `LOAD`        | Whether the unit configuration was loaded successfully |
| `ACTIVE`      | High-level activation state                            |
| `SUB`         | More detailed state of the unit                        |
| `DESCRIPTION` | Human-readable description                             |

A service showing:

```text
active running
```

is currently running successfully.

---

## Step 2 — List All Service Units

To display loaded services regardless of whether they are currently active:

```bash
systemctl list-units --type=service --all
```

This is useful when investigating services that are:

* Running
* Stopped
* Failed
* Inactive

---

# 🧪 Task 2: Check the Status of a Service

Before managing a service, identify its exact unit name.

For example:

```bash
systemctl status ssh
```

On some systems, the unit may be displayed as:

```text
ssh.service
```

### Example Output

```text
● ssh.service - OpenBSD Secure Shell server
     Loaded: loaded (...)
     Active: active (running)
```

### Important Information

The status output can tell you:

* Whether the service is running.
* Whether it starts at boot.
* The process ID.
* Recent service logs.
* Recent start/stop events.
* Whether the service has failed.

---

# 🧪 Task 3: Start a Service

To start a stopped service:

```bash
sudo systemctl start <service-name>
```

### Example

```bash
sudo systemctl start apache2
```

Verify the service:

```bash
systemctl status apache2
```

If successful, you should see something similar to:

```text
Active: active (running)
```

> ⚠️ Only start services that are installed and appropriate for your lab environment.

---

# 🧪 Task 4: Stop a Service

To stop a running service:

```bash
sudo systemctl stop <service-name>
```

### Example

```bash
sudo systemctl stop apache2
```

Verify:

```bash
systemctl status apache2
```

The service should no longer show as actively running.

> ⚠️ Be careful when stopping services on production systems. Stopping critical services such as networking, SSH, or storage-related services can interrupt system access or functionality.

---

# 🧪 Task 5: Enable a Service at Boot

Starting a service **now** does not necessarily mean it will start automatically after reboot.

To enable a service at boot:

```bash
sudo systemctl enable <service-name>
```

### Example

```bash
sudo systemctl enable apache2
```

Verify the boot configuration:

```bash
systemctl is-enabled apache2
```

Expected result:

```text
enabled
```

### 💡 Important Concept

```text
start   → affects the service now
enable  → affects whether it starts automatically at boot
```

These are two different operations.

---

# 🧪 Task 6: Disable a Service at Boot

To prevent a service from starting automatically during boot:

```bash
sudo systemctl disable <service-name>
```

Verify:

```bash
systemctl is-enabled <service-name>
```

Expected result:

```text
disabled
```

### Important

`disable` normally changes the **boot-time configuration**. It does not necessarily stop a service that is already running.

If you want to stop the service immediately as well:

```bash
sudo systemctl stop <service-name>
```

---

# 🧠 Start vs Stop vs Enable vs Disable

Understanding these four commands is essential.

| Command             | Effect                                  |
| ------------------- | --------------------------------------- |
| `systemctl start`   | Starts the service now                  |
| `systemctl stop`    | Stops the service now                   |
| `systemctl enable`  | Configures the service to start at boot |
| `systemctl disable` | Prevents automatic startup at boot      |

### Visual Concept

```text
                 SYSTEMD SERVICE
                       │
          ┌────────────┴────────────┐
          ↓                         ↓
     Current State              Boot State
          │                         │
    ┌─────┴─────┐             ┌─────┴─────┐
    │            │             │           │
   start        stop         enable      disable
    │            │             │           │
    ↓            ↓             ↓           ↓
 Running       Stopped       Starts       Does not
                              at boot      start at boot
```

---

# 🔍 Task 7: Useful Service Inspection Commands

### Check Whether a Service Is Active

```bash
systemctl is-active <service-name>
```

Example:

```bash
systemctl is-active ssh
```

Possible output:

```text
active
```

---

### Check Whether a Service Is Enabled

```bash
systemctl is-enabled <service-name>
```

Possible output:

```text
enabled
```

---

### List Failed Services

```bash
systemctl --failed
```

This is particularly useful during troubleshooting.

---

### List Services That Start During Boot

```bash
systemctl list-unit-files --type=service
```

This displays service unit files and their enablement state.

---

# 🛠️ Practical Troubleshooting Workflow

A basic service troubleshooting process can look like this:

```text
       ┌──────────────────────┐
       │ Service Problem      │
       └──────────┬───────────┘
                  ↓
       ┌──────────────────────┐
       │ Check service status │
       │ systemctl status     │
       └──────────┬───────────┘
                  ↓
       ┌──────────────────────┐
       │ Is service running?  │
       └──────────┬───────────┘
             Yes  │  No
              ↓   │   ↓
        ┌────────┐ ┌──────────────┐
        │Inspect │ │Start service │
        │logs    │ │systemctl     │
        └────────┘ │start         │
                   └──────┬───────┘
                          ↓
                 ┌────────────────┐
                 │ Verify status  │
                 └────────────────┘
```

---

# 📊 Command Reference

| Command                                     | Purpose                           |
| ------------------------------------------- | --------------------------------- |
| `systemctl list-units --type=service`       | List active service units         |
| `systemctl list-units --type=service --all` | List all loaded service units     |
| `systemctl status <service>`                | Display service status            |
| `sudo systemctl start <service>`            | Start a service                   |
| `sudo systemctl stop <service>`             | Stop a service                    |
| `sudo systemctl enable <service>`           | Enable service at boot            |
| `sudo systemctl disable <service>`          | Disable service at boot           |
| `systemctl is-active <service>`             | Check current active state        |
| `systemctl is-enabled <service>`            | Check boot-time configuration     |
| `systemctl --failed`                        | List failed units                 |
| `systemctl list-unit-files --type=service`  | List installed service unit files |

---

# 🔐 Security Perspective

Service management is an important part of Linux security.

Unnecessary services can:

* Consume system resources.
* Increase the system's attack surface.
* Expose unnecessary network functionality.
* Create additional maintenance requirements.

Administrators should understand which services are running and why they are required.

A useful first check is:

```bash
systemctl --type=service --state=running
```

This helps identify services currently running on the system.

> **Security Principle:** Run only the services that are required for the system's intended purpose.

---

# 🚀 Bonus: Restart and Reload

Two additional commands are useful in real-world administration.

### Restart a Service

```bash
sudo systemctl restart <service-name>
```

This stops and starts the service again.

### Reload Configuration

Some services support configuration reloads without completely restarting the service:

```bash
sudo systemctl reload <service-name>
```

This depends on whether the particular service supports reload operations.

---

# 📝 Lab Verification

Run the following commands to verify your understanding:

### 1. List Services

```bash
systemctl list-units --type=service
```

### 2. Check a Service

```bash
systemctl status <service-name>
```

### 3. Start a Service

```bash
sudo systemctl start <service-name>
```

### 4. Verify It

```bash
systemctl is-active <service-name>
```

### 5. Stop the Service

```bash
sudo systemctl stop <service-name>
```

### 6. Enable at Boot

```bash
sudo systemctl enable <service-name>
```

### 7. Verify Boot Configuration

```bash
systemctl is-enabled <service-name>
```

### 8. Disable at Boot

```bash
sudo systemctl disable <service-name>
```

---

# 🎓 Learning Outcomes

After completing this lab, you should be able to:

* Explain the role of systemd.
* Identify systemd-managed services.
* Inspect service states using `systemctl`.
* Start and stop services safely.
* Configure services to start at boot.
* Disable unnecessary automatic service startup.
* Distinguish between **runtime state** and **boot-time configuration**.
* Perform basic service troubleshooting.

---

# 🏆 Real-World DevOps Relevance

Systemd knowledge is highly valuable for Linux and DevOps environments.

In real-world infrastructure, administrators frequently use systemd to manage:

* Web servers
* SSH services
* Monitoring agents
* Application services
* Background workers
* Container-related services
* Security services
* Custom applications

For example, a DevOps engineer may investigate an application outage using:

```bash
systemctl status application.service
```

followed by:

```bash
journalctl -u application.service
```

This combination provides both the **service state** and its associated **logs**.

---

# 🏁 Conclusion

In this lab, you explored **systemd service management** using the `systemctl` command.

You learned how to inspect running services, check service health, start and stop services, and configure whether services should automatically start during system boot.

The most important concept is understanding the difference between **`start`/`stop`** and **`enable`/`disable`**:

```text
start / stop       → Current runtime state
enable / disable   → Boot-time configuration
```

These skills form an essential foundation for **Linux system administration, DevOps, server management, troubleshooting, and security operations**.

---

## 📌 Key Takeaway

> **`systemctl` gives you control over systemd services — helping you inspect, manage, troubleshoot, and automate Linux service behavior.**

---

## 🏅 Lab Status

**Lab 38 — Systemd Services Overview**
**Status:** ✅ Completed
**Focus:** Linux • systemd • systemctl • Service Management • Troubleshooting • DevOps
