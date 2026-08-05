# Lab 29: Scheduling Tasks with `at`

## Overview

This lab demonstrates one-time task scheduling in Linux using the `at` command.

The `at` utility allows administrators and users to schedule commands or scripts to execute once at a specified future time. Unlike `cron`, which is primarily used for recurring tasks, `at` is designed for one-time scheduled execution.

This lab covers installation verification, service management, one-time task scheduling, job monitoring, and scheduled-job removal.

---

## Objectives

- Understand the purpose and functionality of the `at` command.
- Verify the availability of the `at` utility.
- Install `at` when necessary.
- Manage the `atd` service.
- Schedule a one-time task using `at`.
- Monitor scheduled jobs using `atq`.
- Remove scheduled jobs using `atrm`.
- Verify the execution and management of scheduled tasks.

---

## Prerequisites

- Linux-based operating system.
- Basic knowledge of the Linux command line.
- User account with sudo privileges.
- `at` package.
- `atd` service.

---

## Environment

| Item | Details |
|---|---|
| Operating System | Ubuntu 24.04.3 LTS |
| Architecture | x86_64 |
| User | ubuntu |
| Shell | Bash |
| Lab Directory | `Lab-29-Scheduling-Tasks-with-at` |
| Scheduling Tool | `at` |
| Service | `atd` |

> Command outputs in this document are based on the actual lab environment and are recorded after completing each task.

---

# Task 1: Install and Verify `at`

## 1.1 Check Whether `at` Is Installed

The `at` command was checked using:

```bash
command -v at
