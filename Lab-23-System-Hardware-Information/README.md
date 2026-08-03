# Lab 23: System Hardware Information

## 📌 Overview

This lab focuses on gathering and interpreting basic system hardware and resource information in a Linux environment.

Linux provides several built-in command-line utilities that allow system administrators and DevOps engineers to inspect CPU architecture, memory utilization, and filesystem storage without requiring additional graphical tools.

The commands practiced in this lab are:

- `lscpu` — CPU and processor architecture information
- `free` — RAM and swap memory information
- `df` — Filesystem and disk-space utilization

---

## 🎯 Objectives

By completing this lab, I learned how to:

- Retrieve detailed CPU information from a Linux system.
- Identify CPU architecture, cores, threads, and virtualization information.
- Monitor RAM and swap utilization.
- Analyze filesystem disk usage.
- Interpret system resource information from the Linux command line.
- Use command output for basic system monitoring and troubleshooting.

---

## 🖥️ Environment

| Component | Details |
|---|---|
| Operating System | Ubuntu Linux |
| Architecture | x86_64 |
| Environment | Linux CLI |
| Access | Remote Linux system |
| Shell | Bash |

---

# Task 1: Display CPU Information

## Command Used

```bash
lscpu
