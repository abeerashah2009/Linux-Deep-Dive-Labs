# 🔥 Lab 39: Basic Firewall Setup with UFW

> **Linux Security | Firewall Configuration | Network Access Control | UFW**

---

## 📌 Lab Overview

A **firewall** is a critical security control used to regulate network traffic entering and leaving a system.

In this lab, you will configure **UFW (Uncomplicated Firewall)** on a Linux system and learn how to control incoming network connections using simple firewall rules.

The lab covers the complete basic workflow:

```text
Install → Enable → Configure → Verify → Test
```

This provides practical experience with a fundamental Linux security mechanism and builds a foundation for more advanced firewall technologies such as **iptables** and **nftables**.

---

## 🎯 Objectives

By completing this lab, you will learn how to:

* Understand the purpose of a firewall.
* Install UFW on Ubuntu/Linux.
* Enable and disable the UFW firewall.
* Configure basic inbound firewall rules.
* Allow SSH and HTTP traffic.
* View and verify firewall rules.
* Test how firewall rules affect network connectivity.
* Understand the security importance of controlling inbound traffic.

---

## 🛠️ Prerequisites

* Basic Linux command-line knowledge.
* Ubuntu or another Linux distribution supporting UFW.
* Terminal or SSH access.
* `sudo` privileges.
* Basic understanding of IP addresses, ports, and network services.

---

# 🧠 1. Understanding Firewalls

A **firewall** controls network traffic according to predefined security rules.

A simple firewall can be thought of as a security gate:

```text
                    INTERNET
                       │
                       ▼
              ┌─────────────────┐
              │     FIREWALL    │
              │      UFW        │
              └────────┬────────┘
                       │
             ┌─────────┴─────────┐
             │                   │
          ALLOWED              DENIED
             │                   │
             ▼                   X
        Linux Server       Unauthorized Traffic
```

### Why Firewalls Matter

Firewalls can help:

* Reduce the attack surface.
* Restrict unauthorized access.
* Control which network services are reachable.
* Protect servers from unnecessary inbound connections.
* Enforce network security policies.

---

# 🛡️ 2. What is UFW?

**UFW** stands for **Uncomplicated Firewall**.

It provides a simplified interface for managing firewall rules on Linux systems.

Instead of manually constructing complex firewall configurations, administrators can use straightforward commands such as:

```bash
sudo ufw allow ssh
```

and:

```bash
sudo ufw deny 23
```

---

# 🧪 Task 1: Install UFW

## Step 1 — Update Package Information

Before installing packages, update the package index:

```bash
sudo apt update
```

This retrieves the latest available package information from the configured repositories.

---

## Step 2 — Install UFW

```bash
sudo apt install ufw
```

If UFW is already installed, the package manager will indicate that no installation is required.

### Verify Installation

```bash
ufw version
```

Example:

```text
ufw 0.x.x
```

---

# 🧪 Task 2: Enable UFW

Before enabling the firewall, **make sure SSH access is allowed if you are connected remotely**.

Allow SSH first:

```bash
sudo ufw allow ssh
```

Then enable UFW:

```bash
sudo ufw enable
```

You may see a warning similar to:

```text
Command may disrupt existing ssh connections.
Proceed with operation (y|n)?
```

Enter:

```text
y
```

---

## 🔎 Verify Firewall Status

```bash
sudo ufw status
```

Expected output should indicate:

```text
Status: active
```

---

# 🧪 Task 3: Configure Basic Firewall Rules

## Step 1 — Allow SSH

SSH normally uses **TCP port 22**.

Allow SSH by service name:

```bash
sudo ufw allow ssh
```

You can also specify the port:

```bash
sudo ufw allow 22/tcp
```

### Why SSH Is Important

SSH provides secure remote access to Linux servers.

> ⚠️ **Important:** If you are connected to the server through SSH, do not enable UFW before ensuring that SSH is permitted. Otherwise, you may accidentally lock yourself out.

---

## Step 2 — Allow HTTP

HTTP normally uses **TCP port 80**.

Allow HTTP:

```bash
sudo ufw allow http
```

Or:

```bash
sudo ufw allow 80/tcp
```

This allows incoming HTTP traffic for services such as a web server.

---

## Step 3 — View the Rules

```bash
sudo ufw status
```

You may see:

```text
Status: active

To                         Action      From
--                         ------      ----
22/tcp                     ALLOW       Anywhere
80/tcp                     ALLOW       Anywhere
```

---

# 🧪 Task 4: Check Detailed UFW Status

For more information, run:

```bash
sudo ufw status verbose
```

This provides information about:

* Firewall status
* Default incoming policy
* Default outgoing policy
* Logging
* Active rules

A typical configuration may look like:

```text
Status: active
Logging: on
Default: deny (incoming), allow (outgoing)
```

---

# 🔐 Understanding Default Policies

A common server firewall configuration is:

```text
Incoming  → DENY
Outgoing  → ALLOW
```

This means unsolicited incoming connections are blocked unless an explicit rule allows them.

Conceptually:

```text
                 Incoming Traffic
                        │
                        ▼
                ┌───────────────┐
                │      UFW      │
                └───────┬───────┘
                        │
              ┌─────────┴─────────┐
              ↓                   ↓
          Allowed              Denied
              │                   │
              ↓                   X
          SSH / HTTP        Unapproved Ports
```

Check the current defaults with:

```bash
sudo ufw status verbose
```

---

# 🧪 Task 5: Test Firewall Rules

## Step 1 — Test SSH Access

From another machine, attempt to connect to the server:

```bash
ssh username@server-ip
```

If SSH is allowed and the service is running, the connection should succeed.

---

## Step 2 — Temporarily Deny SSH

> ⚠️ **WARNING:** Do not perform this step on a remote production server unless you have another way to access the system.

Deny SSH:

```bash
sudo ufw deny ssh
```

Check the rule:

```bash
sudo ufw status
```

You should see a rule denying SSH traffic.

---

## Step 3 — Restore SSH Access

Immediately restore the SSH rule:

```bash
sudo ufw delete deny ssh
```

Then allow SSH again:

```bash
sudo ufw allow ssh
```

Verify:

```bash
sudo ufw status
```

---

# 🔍 Useful UFW Commands

## Show Firewall Status

```bash
sudo ufw status
```

## Show Detailed Status

```bash
sudo ufw status verbose
```

## Allow a Service

```bash
sudo ufw allow ssh
```

## Allow a Port

```bash
sudo ufw allow 8080/tcp
```

## Deny a Port

```bash
sudo ufw deny 23/tcp
```

## Delete a Rule

```bash
sudo ufw delete deny 23/tcp
```

## Reload UFW

```bash
sudo ufw reload
```

## Disable UFW

```bash
sudo ufw disable
```

> ⚠️ Disabling the firewall removes the protection provided by its active rules. Use this only when appropriate for your lab or troubleshooting scenario.

---

# 🧩 Advanced Rule Examples

## Allow HTTPS

HTTPS normally uses TCP port 443:

```bash
sudo ufw allow https
```

Or:

```bash
sudo ufw allow 443/tcp
```

---

## Allow a Specific Port

For example:

```bash
sudo ufw allow 8080/tcp
```

---

## Allow Access from a Specific IP

```bash
sudo ufw allow from 192.168.1.100
```

This is more restrictive than allowing traffic from anywhere.

---

## Allow SSH Only from a Specific Network

For example:

```bash
sudo ufw allow from 192.168.1.0/24 to any port 22 proto tcp
```

This allows SSH connections only from the specified network.

---

# 🧠 Firewall Rule Design

A secure firewall configuration should follow the principle of **least privilege**.

Instead of allowing every incoming connection:

```text
❌ Allow everything
```

allow only the services that are actually required:

```text
✅ SSH
✅ HTTPS
❌ Unused services
❌ Unnecessary ports
```

### Example Server

A basic web server might require:

| Service         |    Port | Protocol | Access  |
| --------------- | ------: | -------- | ------- |
| SSH             |      22 | TCP      | Allowed |
| HTTP            |      80 | TCP      | Allowed |
| HTTPS           |     443 | TCP      | Allowed |
| Unused services | Various | Various  | Denied  |

---

# 🔐 Security Best Practices

### 1. Allow Required Services Only

Do not open ports simply because they are available.

### 2. Protect SSH

Avoid unnecessarily exposing SSH to the entire internet when access can be restricted.

### 3. Use Least Privilege

Allow only the traffic required for the system's purpose.

### 4. Verify Rules After Changes

Always check:

```bash
sudo ufw status verbose
```

### 5. Be Careful with Remote Servers

Incorrect firewall rules can terminate your remote access.

Always verify that your management connection, such as SSH, is permitted before enabling the firewall.

---

# 🚀 DevOps & Cloud Relevance

Firewall configuration is an important skill for **DevOps and cloud infrastructure**.

In cloud environments, network access may be controlled by multiple layers:

```text
Internet
   │
   ▼
Cloud Network Controls
   │
   ▼
Security Groups / Network ACLs
   │
   ▼
Linux Firewall
   │
   ▼
Application
```

For example, an AWS EC2 server may have:

* AWS Security Group rules
* Network ACL rules
* UFW rules
* Application-level access controls

Understanding how these layers interact is essential when troubleshooting connectivity.

---

# 🧪 Lab Verification Checklist

Run the following commands to verify the lab.

### Check UFW Installation

```bash
ufw version
```

### Allow SSH

```bash
sudo ufw allow ssh
```

### Allow HTTP

```bash
sudo ufw allow http
```

### Enable Firewall

```bash
sudo ufw enable
```

### Check Status

```bash
sudo ufw status
```

### Check Detailed Configuration

```bash
sudo ufw status verbose
```

### Verify Rules

```bash
sudo ufw status numbered
```

---

# 📊 Command Reference

| Command                    | Purpose                        |
| -------------------------- | ------------------------------ |
| `sudo apt update`          | Update package information     |
| `sudo apt install ufw`     | Install UFW                    |
| `ufw version`              | Display UFW version            |
| `sudo ufw enable`          | Enable firewall                |
| `sudo ufw disable`         | Disable firewall               |
| `sudo ufw status`          | Display firewall status        |
| `sudo ufw status verbose`  | Display detailed configuration |
| `sudo ufw status numbered` | Display numbered rules         |
| `sudo ufw allow ssh`       | Allow SSH                      |
| `sudo ufw allow http`      | Allow HTTP                     |
| `sudo ufw allow 443/tcp`   | Allow HTTPS                    |
| `sudo ufw deny <port>`     | Deny traffic                   |
| `sudo ufw delete <rule>`   | Remove a rule                  |
| `sudo ufw reload`          | Reload firewall configuration  |

---

# 🎓 Learning Outcomes

After completing this lab, you should be able to:

* Explain what a firewall does.
* Describe the purpose of UFW.
* Install and enable UFW.
* Configure inbound firewall rules.
* Allow SSH and HTTP traffic.
* Inspect active firewall rules.
* Test firewall behavior.
* Understand default deny/allow policies.
* Apply the principle of least privilege to network access.
* Recognize the importance of firewall configuration in Linux and cloud environments.

---

# 🏁 Conclusion

In this lab, you configured **UFW (Uncomplicated Firewall)** to control network traffic on a Linux system.

You learned how to install and enable UFW, allow required services such as **SSH and HTTP**, inspect firewall rules, and test how access changes when rules are modified.

The lab also introduced an important security principle:

> **Allow only the network traffic that is required, and deny unnecessary access.**

Firewall management is a fundamental Linux security skill and an important part of **server hardening, DevOps, cloud security, and infrastructure administration**.

---

## 📌 Key Takeaway

```text
                SECURE LINUX SERVER
                        │
                        ▼
                     UFW
                        │
          ┌─────────────┴─────────────┐
          ▼                           ▼
      Required Traffic          Unnecessary Traffic
          │                           │
       ALLOW                         DENY
          │                           │
          ▼                           ▼
    SSH / HTTP / HTTPS         Unauthorized Access
```

> 🔥 **UFW provides a simple way to control network access — but a firewall is only effective when its rules are carefully designed, verified, and maintained.**

---

## 🏆 Lab Status

**Lab 39 — Basic Firewall Setup (UFW)**
**Status:** ✅ Completed
**Focus:** Linux Security • UFW • Firewall Configuration • Network Access Control • Server Hardening • DevOps
