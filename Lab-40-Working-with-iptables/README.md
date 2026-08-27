# 🔥 Lab 40: Working with `iptables`

> **Linux Security | Firewall Management | Packet Filtering | Network Access Control**

---

## 📌 Lab Overview

`iptables` is a powerful Linux command-line utility used to configure and inspect the kernel's **packet-filtering firewall rules**.

In this lab, you will work with `iptables` to inspect existing firewall rules, create rules that allow or block specific network traffic, and save the configuration for persistence.

This lab builds practical knowledge of **Linux firewall administration, network security, server hardening, and traffic control**.

---

## 🎯 Objectives

By completing this lab, you will learn how to:

* List existing `iptables` rules.
* Understand `iptables` chains and policies.
* Create rules for specific TCP ports.
* Allow incoming network traffic.
* Block incoming network traffic.
* Understand rule targets such as `ACCEPT` and `DROP`.
* Save the current firewall configuration.
* Understand why persistent firewall configuration matters.

---

## 🛠️ Prerequisites

* Basic Linux command-line knowledge.
* Access to a Linux system.
* `iptables` installed.
* `sudo` or root privileges.
* Basic understanding of:

  * TCP/IP
  * Ports
  * Network services
  * Firewall concepts

---

# 📚 1. Understanding `iptables`

`iptables` provides an interface for configuring packet-filtering rules on Linux.

At a simplified level:

```text id="2u8k9c"
                 Incoming Packet
                       │
                       ▼
                ┌─────────────┐
                │   iptables  │
                │   INPUT     │
                └──────┬──────┘
                       │
              ┌────────┴────────┐
              ▼                 ▼
           ACCEPT              DROP
              │                 │
              ▼                 X
          Application       Packet Blocked
```

The firewall examines packets and applies rules to determine what should happen to them.

---

# 🧠 2. Understanding iptables Chains

Some of the most important built-in chains are:

| Chain     | Purpose                                           |
| --------- | ------------------------------------------------- |
| `INPUT`   | Handles packets destined for the local system     |
| `OUTPUT`  | Handles packets originating from the local system |
| `FORWARD` | Handles packets being routed through the system   |

A simplified traffic model:

```text id="y6u9n4"
                    Network Traffic
                          │
             ┌────────────┴────────────┐
             ▼                         ▼
       Incoming Traffic           Outgoing Traffic
             │                         │
             ▼                         ▼
          INPUT                     OUTPUT
             │                         │
             └──────────┬──────────────┘
                        │
                     System
```

---

# 🧪 Task 1: List Existing `iptables` Rules

## Step 1 — Display Current Rules

Run:

```bash id="f4z7wu"
sudo iptables -L
```

This lists the rules in the default filter table.

---

## Step 2 — Display Rules with More Useful Details

For easier analysis, use:

```bash id="4fwh9r"
sudo iptables -L -n -v
```

### Option Breakdown

| Option | Purpose                                       |
| ------ | --------------------------------------------- |
| `-L`   | List rules                                    |
| `-n`   | Do not resolve IP addresses or ports to names |
| `-v`   | Display verbose information                   |

This version can provide useful information such as:

* Packet counters
* Byte counters
* Interfaces
* Protocols
* Sources
* Destinations
* Targets

---

## Example Output

```text id="4gk7u8"
Chain INPUT (policy ACCEPT)
target     prot opt source      destination

Chain FORWARD (policy ACCEPT)
target     prot opt source      destination

Chain OUTPUT (policy ACCEPT)
target     prot opt source      destination
```

### 🔎 What to Observe

Pay attention to:

* Chain names
* Default policies
* Protocols
* Source addresses
* Destination addresses
* Targets such as `ACCEPT` or `DROP`

---

# 🧪 Task 2: Allow Incoming HTTP Traffic

HTTP commonly uses **TCP port 80**.

Add a rule to allow incoming HTTP traffic:

```bash id="r0p0wl"
sudo iptables -A INPUT -p tcp --dport 80 -j ACCEPT
```

---

## 🔍 Understanding the Command

```text id="f8s4d0"
-A INPUT
```

Appends the rule to the `INPUT` chain.

```text id="dbyy5j"
-p tcp
```

Matches TCP traffic.

```text id="4kq5ht"
--dport 80
```

Matches packets destined for TCP port 80.

```text id="zj3cvk"
-j ACCEPT
```

Accepts matching packets.

### Complete Logic

```text id="hm0fpl"
Incoming TCP Packet
        │
        ▼
Destination Port 80?
        │
     ┌──┴──┐
    YES    NO
     │      │
     ▼      ▼
  ACCEPT   Continue
```

---

# 🧪 Task 3: Verify the New Rule

After adding the rule:

```bash id="i8c4c2"
sudo iptables -L -n -v
```

You should see a rule similar to:

```text id="km3n8x"
ACCEPT  tcp  --  0.0.0.0/0  0.0.0.0/0  tcp dpt:80
```

You can also display rules with line numbers:

```bash id="8i1ry4"
sudo iptables -L INPUT --line-numbers
```

Line numbers are useful when identifying or removing specific rules.

---

# 🧪 Task 4: Block a Specific Port

To block incoming traffic to a TCP port, use the `DROP` target.

For example, to block TCP port 8080:

```bash id="g4ef8e"
sudo iptables -A INPUT -p tcp --dport 8080 -j DROP
```

### Understanding `DROP`

`DROP` silently discards matching packets.

```text id="prm8cj"
Incoming TCP Traffic
        │
        ▼
   Port 8080?
        │
       YES
        │
        ▼
      DROP
        │
        X
   Packet discarded
```

---

# ⚠️ ACCEPT vs DROP

| Target   | Behavior                                          |
| -------- | ------------------------------------------------- |
| `ACCEPT` | Allows the packet                                 |
| `DROP`   | Silently discards the packet                      |
| `REJECT` | Rejects the packet and generally sends a response |

For example:

### Allow HTTP

```bash id="8cg5jy"
sudo iptables -A INPUT -p tcp --dport 80 -j ACCEPT
```

### Drop TCP Port 8080

```bash id="wxq44s"
sudo iptables -A INPUT -p tcp --dport 8080 -j DROP
```

---

# 🧪 Task 5: Save the `iptables` Configuration

Rules added directly with `iptables` are generally runtime configuration and may not automatically survive a reboot.

Save the current rules:

```bash id="n9t7qk"
sudo iptables-save > /etc/iptables/rules.v4
```

This stores the current rules in:

```text id="xg3r9h"
/etc/iptables/rules.v4
```

### Verify the Saved Configuration

```bash id="2d1e9c"
sudo cat /etc/iptables/rules.v4
```

You should see the saved firewall configuration.

---

# 🔐 Important: Rule Persistence

Saving rules to a file does not, by itself, guarantee that the rules will automatically be restored after reboot on every Linux distribution.

The persistence mechanism depends on the operating system and installed firewall packages.

On Ubuntu/Debian systems, a common approach is the `iptables-persistent` package:

```bash id="x5j5tq"
sudo apt install iptables-persistent
```

After installation, rules can be saved with:

```bash id="w3o9l8"
sudo netfilter-persistent save
```

And restored with:

```bash id="qj1x3a"
sudo netfilter-persistent reload
```

> **Note:** Package availability and firewall architecture can vary between Linux distributions.

---

# 🧪 Task 6: Inspect Saved Rules

Display the saved configuration:

```bash id="g7qz2v"
sudo iptables-save
```

You can also redirect the output to a file:

```bash id="6v3j3e"
sudo iptables-save > /etc/iptables/rules.v4
```

Compare the active configuration with the saved configuration when troubleshooting persistence issues.

---

# 🔎 Useful `iptables` Commands

## List Rules

```bash id="kj4q3p"
sudo iptables -L
```

## List Rules with Counters

```bash id="4g3y1n"
sudo iptables -L -n -v
```

## List Rules with Line Numbers

```bash id="uj5r5u"
sudo iptables -L --line-numbers
```

## Display Rules in Save Format

```bash id="5j9n1a"
sudo iptables-save
```

## Add an HTTP Rule

```bash id="0s4l0p"
sudo iptables -A INPUT -p tcp --dport 80 -j ACCEPT
```

## Add a Drop Rule

```bash id="j5z8jh"
sudo iptables -A INPUT -p tcp --dport 8080 -j DROP
```

---

# 🗑️ Removing a Rule

If you need to remove a rule, first display its line number:

```bash id="j2wqhv"
sudo iptables -L INPUT --line-numbers
```

Example:

```text id="jtpq0m"
num  target   prot opt source      destination
1    ACCEPT   tcp  --  0.0.0.0/0   0.0.0.0/0   tcp dpt:80
2    DROP     tcp  --  0.0.0.0/0   0.0.0.0/0   tcp dpt:8080
```

Remove rule number 2:

```bash id="v1q7z7"
sudo iptables -D INPUT 2
```

Then verify:

```bash id="l3e6yb"
sudo iptables -L -n -v
```

> ⚠️ Rule numbers can change after deleting rules, so always check the current numbering before removing another rule.

---

# 🧠 Understanding Rule Order

`iptables` processes rules **in order**.

For example:

```text id="6q7j7u"
Rule 1 → ACCEPT port 80
Rule 2 → DROP port 80
```

A packet matching port 80 may be accepted by the first rule and never reach the second rule.

Therefore:

> **Rule order matters.**

Think of the rules as a checklist processed from top to bottom.

```text id="5p1cif"
Packet
  │
  ▼
Rule 1 ── Match? ── YES ──► Action
  │
  NO
  │
  ▼
Rule 2 ── Match? ── YES ──► Action
  │
  NO
  │
  ▼
Next Rule
```

---

# 🔐 Security Considerations

Firewall rules should follow the principle of **least privilege**.

Instead of opening unnecessary ports:

```text id="z4z0yj"
❌ Unnecessary ports
❌ Unused services
❌ Broad access
```

prefer:

```text id="2a0d07"
✅ Required ports
✅ Required services
✅ Restricted sources where possible
```

For example, a web server might require:

| Service | Port | Protocol |
| ------- | ---: | -------- |
| SSH     |   22 | TCP      |
| HTTP    |   80 | TCP      |
| HTTPS   |  443 | TCP      |

Other ports should remain restricted unless there is a legitimate requirement.

---

# ⚠️ Remote Server Warning

Be especially careful when modifying firewall rules on a remote server.

If you are connected through SSH and accidentally block SSH traffic, you may lose remote access.

Before applying firewall changes, make sure you understand:

```text id="v7z9oj"
Current SSH Connection
        ↓
SSH Port Allowed?
        ↓
Firewall Rule Applied
        ↓
Connectivity Verified
```

For cloud environments, remember that firewall rules may exist at multiple layers.

```text id="e9uj7f"
Internet
   │
   ▼
Cloud Firewall / Security Group
   │
   ▼
Network Controls
   │
   ▼
Linux iptables
   │
   ▼
Application
```

---

# 🚀 DevOps & Cloud Relevance

Understanding `iptables` is valuable for Linux administration and DevOps because firewall rules can affect:

* SSH connectivity
* Web applications
* APIs
* Database connections
* Container networking
* Monitoring systems
* Service-to-service communication

When an application cannot connect to another service, firewall rules should be included in the troubleshooting process.

Useful commands include:

```bash id="84m1cg"
sudo iptables -L -n -v
```

and:

```bash id="b5dj1f"
sudo iptables-save
```

These commands help administrators understand the active packet-filtering configuration.

---

# 🧪 Lab Verification Checklist

Use the following commands to verify the lab.

### 1. List Existing Rules

```bash id="5e7p2n"
sudo iptables -L -n -v
```

### 2. Allow HTTP

```bash id="x5ym6y"
sudo iptables -A INPUT -p tcp --dport 80 -j ACCEPT
```

### 3. Verify HTTP Rule

```bash id="u1l6s8"
sudo iptables -L INPUT --line-numbers
```

### 4. Add a Test Drop Rule

```bash id="3e7z3d"
sudo iptables -A INPUT -p tcp --dport 8080 -j DROP
```

### 5. Verify Rules

```bash id="h9m8jh"
sudo iptables -L -n -v
```

### 6. Save Configuration

```bash id="3u5pvi"
sudo iptables-save > /etc/iptables/rules.v4
```

### 7. Verify Saved Rules

```bash id="r5q8r3"
sudo cat /etc/iptables/rules.v4
```

---

# 📊 Command Reference

| Command                                       | Purpose                           |
| --------------------------------------------- | --------------------------------- |
| `sudo iptables -L`                            | List firewall rules               |
| `sudo iptables -L -n -v`                      | Display detailed rules            |
| `sudo iptables -L --line-numbers`             | Display rules with numbers        |
| `sudo iptables -A INPUT ...`                  | Append a rule                     |
| `-p tcp`                                      | Match TCP traffic                 |
| `--dport 80`                                  | Match destination port 80         |
| `-j ACCEPT`                                   | Allow matching traffic            |
| `-j DROP`                                     | Silently discard matching traffic |
| `sudo iptables -D INPUT <number>`             | Delete a rule                     |
| `sudo iptables-save`                          | Export current rules              |
| `sudo iptables-save > /etc/iptables/rules.v4` | Save rules to a file              |

---

# 🎓 Learning Outcomes

After completing this lab, you should be able to:

* Explain the purpose of `iptables`.
* Identify the major `iptables` chains.
* List and inspect existing firewall rules.
* Understand firewall policies and rule targets.
* Create rules for specific TCP ports.
* Allow or block incoming traffic.
* Understand why rule order matters.
* Save the current firewall configuration.
* Recognize the importance of firewall persistence.
* Apply basic firewall concepts to Linux and cloud environments.

---

# 🏁 Conclusion

In this lab, you gained practical experience with **`iptables`**, one of the foundational firewall technologies in Linux.

You learned how to inspect existing rules, create rules for specific ports, allow or block traffic, understand chains and targets, and save firewall configurations.

The most important concepts from this lab are:

```text id="w6q1ny"
iptables
   │
   ├── INPUT   → Incoming traffic
   ├── OUTPUT  → Outgoing traffic
   └── FORWARD → Routed traffic
```

and:

```text id="m4x9gc"
ACCEPT → Allow traffic
DROP   → Discard traffic
```

> **Key Takeaway:**
> **Effective firewall administration requires understanding traffic direction, rule order, allowed services, and persistent configuration.**

---

## 🏆 Lab Status

**Lab 40 — Working with `iptables`**
**Status:** ✅ Completed
**Focus:** Linux Firewall • `iptables` • Packet Filtering • Network Security • Server Hardening • DevOps
