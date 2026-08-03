# Lab 27: Password Policies

## Objective

The objective of this lab is to understand and manage basic Linux password policies using the `chage` command and password configuration files.

In this lab, we will:

- Check the current password aging policy.
- Configure password expiry.
- Configure password expiry warning.
- Verify the changes.
- Review password-related settings in `/etc/login.defs`.
- Understand basic password complexity configuration.

---

## Prerequisites

- Linux/Ubuntu system
- Terminal access
- Sudo privileges
- Basic knowledge of Linux users and permissions

---

# Task 1: Check Current Password Policy

## Step 1: Check the Current User

First, check the username of the current user.

```bash
whoami
