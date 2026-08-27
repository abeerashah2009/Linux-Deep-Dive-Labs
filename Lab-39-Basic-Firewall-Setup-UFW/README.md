# Lab 39: Basic Firewall Setup with UFW

## Overview

This lab demonstrates the installation, configuration, verification, and basic testing of **UFW (Uncomplicated Firewall)** on Ubuntu Linux.

UFW provides a simplified interface for managing Linux firewall rules through the underlying Netfilter framework.

The lab focuses on:

- Installing and verifying UFW
- Enabling the firewall
- Configuring default firewall policies
- Allowing SSH traffic
- Allowing HTTP traffic
- Verifying firewall rules
- Checking SSH service availability
- Testing temporary firewall rule changes
- Capturing configuration and system evidence
- Documenting the final firewall configuration

---

## Objectives

- Understand the purpose of a host-based firewall.
- Learn the basic UFW command structure.
- Install and verify UFW on Ubuntu.
- Enable UFW safely on a remote Linux server.
- Configure SSH and HTTP firewall rules.
- Verify active firewall rules.
- Understand default incoming and outgoing policies.
- Test firewall rule modification.
- Restore the intended firewall configuration.
- Preserve command output as lab evidence.

---

## Prerequisites

- Ubuntu Linux system
- Sudo privileges
- Basic Linux command-line knowledge
- SSH access to the system
- Basic understanding of TCP/IP networking

---

## Environment

| Item | Details |
|---|---|
| Operating System | Ubuntu Linux |
| Firewall | UFW |
| UFW Version | 0.36.2 |
| SSH Service | OpenSSH |
| SSH Port | 22/TCP |
| HTTP Port | 80/TCP |
| Firewall Logging | Low |
| Default Incoming Policy | Deny |
| Default Outgoing Policy | Allow |
| Default Routed Policy | Deny |

---

# Task 1: Install UFW

## 1. Update Package Lists

The package repository information was refreshed before installing or verifying UFW.

```bash
sudo apt update
```

### Result

The package lists were successfully updated.

---

## 2. Verify UFW Installation

```bash
ufw --version
```

### Output

```text
ufw 0.36.2
Copyright 2008-2023 Canonical Ltd.
```

UFW was already installed on the system.

Evidence:

```text
evidence/ufw-version.txt
```

---

# Task 2: Verify Initial Firewall State

Before enabling the firewall, its initial state was checked:

```bash
sudo ufw status
```

### Result

```text
Status: inactive
```

This confirmed that UFW was installed but initially inactive.

---

# Task 3: Configure SSH Access

Because this system is accessed remotely through SSH, SSH access was explicitly allowed before enabling UFW.

```bash
sudo ufw allow ssh
```

UFW created the following rule:

```text
22/tcp ALLOW IN Anywhere
22/tcp (v6) ALLOW IN Anywhere (v6)
```

SSH service verification:

```bash
systemctl is-active ssh
```

### Result

```text
active
```

The SSH daemon was confirmed to be running.

SSH listening ports were also verified:

```bash
sudo ss -tlnp | grep ':22'
```

The system was listening on TCP port 22 for both IPv4 and IPv6.

Evidence:

```text
evidence/ssh-status.txt
evidence/listening-ports.txt
```

---

# Task 4: Allow HTTP Traffic

HTTP traffic was allowed using:

```bash
sudo ufw allow http
```

This created:

```text
80/tcp ALLOW IN Anywhere
80/tcp (v6) ALLOW IN Anywhere (v6)
```

The firewall rule was later verified using:

```bash
sudo ufw status numbered
```

---

# Task 5: Enable UFW

The firewall was enabled with:

```bash
sudo ufw enable
```

UFW displayed:

```text
Firewall is active and enabled on system startup
```

This confirms that:

- UFW is currently active.
- UFW will start automatically during system startup.

---

# Task 6: Verify Firewall Configuration

The detailed firewall configuration was checked with:

```bash
sudo ufw status verbose
```

### Final Configuration

```text
Status: active
Logging: on (low)
Default: deny (incoming), allow (outgoing), deny (routed)

To                         Action      From
--                         ------      ----
22/tcp                     ALLOW IN    Anywhere
80/tcp                     ALLOW IN    Anywhere
22/tcp (v6)                ALLOW IN    Anywhere (v6)
80/tcp                     ALLOW IN    Anywhere (v6)
```

### Security Interpretation

The firewall follows a restrictive inbound policy:

```text
Incoming  → DENY by default
Outgoing  → ALLOW by default
Routed    → DENY by default
```

Only explicitly permitted inbound services are allowed.

Current permitted services:

- SSH — TCP/22
- HTTP — TCP/80

Evidence:

```text
evidence/ufw-status.txt
evidence/ufw-rules.txt
```

---

# Task 7: Verify Numbered Rules

The active firewall rules were displayed with:

```bash
sudo ufw status numbered
```

### Result

```text
[ 1] 22/tcp                     ALLOW IN    Anywhere
[ 2] 80/tcp                     ALLOW IN    Anywhere
[ 3] 22/tcp (v6)                ALLOW IN    Anywhere (v6)
[ 4] 80/tcp                     ALLOW IN    Anywhere (v6)
```

The numbered rules provide an easy way to identify and manage individual firewall rules.

---

# Task 8: Verify SSH Service

The SSH service was checked using:

```bash
systemctl status ssh --no-pager
```

### Result

The SSH service was:

```text
Active: active (running)
```

The SSH daemon was listening on:

```text
0.0.0.0:22
[::]:22
```

This confirms that SSH was operational while the firewall was active.

Evidence:

```text
evidence/ssh-status.txt
```

---

# Task 9: Test Temporary SSH Blocking

To demonstrate the effect of a firewall rule, SSH access was temporarily denied:

```bash
sudo ufw deny ssh
```

The resulting configuration showed:

```text
22/tcp DENY IN Anywhere
```

This demonstrated how UFW rules can change the treatment of incoming SSH traffic.

Because the system was being accessed remotely, the SSH deny rule was immediately reversed.

---

# Task 10: Restore SSH Access

SSH access was restored with:

```bash
sudo ufw allow ssh
```

Final verification:

```bash
sudo ufw status numbered
```

### Final SSH Rule

```text
22/tcp ALLOW IN Anywhere
22/tcp (v6) ALLOW IN Anywhere (v6)
```

The temporary deny rule was successfully removed/replaced.

---

# Task 11: Verify Listening Network Services

Listening TCP sockets were examined using:

```bash
sudo ss -tlnp
```

The system confirmed that SSH was listening on TCP port 22.

Other locally running services were also visible, demonstrating the importance of explicitly controlling externally accessible ports through firewall rules.

Evidence:

```text
evidence/listening-ports.txt
```

---

# Evidence Directory

The following evidence files were collected:

```text
evidence/
├── listening-ports.txt
├── ssh-status.txt
├── ufw-rules.txt
├── ufw-status.txt
└── ufw-version.txt
```

### Evidence Description

| File | Purpose |
|---|---|
| `ufw-version.txt` | Records installed UFW version |
| `ufw-status.txt` | Records detailed firewall configuration |
| `ufw-rules.txt` | Records numbered firewall rules |
| `ssh-status.txt` | Records SSH service status |
| `listening-ports.txt` | Records active listening TCP sockets |

---

# Security Considerations

Enabling a firewall on a remote server requires caution.

Before enabling UFW, SSH access should be explicitly allowed:

```bash
sudo ufw allow ssh
```

Otherwise, the administrator may unintentionally block remote access.

The final configuration uses a **default-deny inbound policy**, which follows the security principle of allowing only required services.

Only required services should be exposed.

---

# Verification Checklist

- [x] UFW installed
- [x] UFW version verified
- [x] Initial firewall state checked
- [x] SSH rule configured
- [x] HTTP rule configured
- [x] UFW enabled
- [x] Firewall enabled at system startup
- [x] Default incoming policy verified
- [x] Default outgoing policy verified
- [x] Firewall rules verified
- [x] SSH service verified
- [x] SSH listening port verified
- [x] Temporary SSH deny rule tested
- [x] SSH access rule restored
- [x] Evidence files created
- [x] Final firewall configuration verified

---

# Final Result

The Ubuntu system was successfully configured with UFW.

The final firewall configuration provides:

```text
Firewall Status: ACTIVE
Incoming Policy: DENY
Outgoing Policy: ALLOW
Routed Policy: DENY

Allowed Services:
- SSH 22/TCP
- HTTP 80/TCP
```

The configuration was verified after testing and the final state was restored to the intended secure configuration.

---

## Conclusion

This lab demonstrated the fundamentals of host-based firewall management using UFW.

The practical work covered firewall installation, activation, rule configuration, service verification, temporary rule testing, and evidence collection.

The final configuration follows a basic least-exposure approach by denying unsolicited inbound traffic while explicitly allowing required services such as SSH and HTTP.
