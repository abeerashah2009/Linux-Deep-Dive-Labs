# ⏱️ Lab 53: Checking System Uptime

> **Linux System Administration | Uptime Monitoring | Load Average Analysis**

## 📌 Lab Overview

In this lab, I learned how to check Linux system uptime, identify the last boot time, view active login sessions, and understand system load averages.

I also generated temporary CPU load and observed how Linux load averages changed.

**Operating System:** Ubuntu 24.04
**Environment:** AWS EC2
**CPU Count:** 2

---

## 🎯 Objectives

- Determine system uptime using Linux commands.
- Identify the system boot date and time.
- Check currently logged-in users.
- Understand 1-minute, 5-minute, and 15-minute load averages.
- Read load average information from `/proc/loadavg`.
- Generate temporary CPU load and observe its effect.

---

## 1. Verify User and Hostname

### Commands

```bash
whoami
hostname
```

### Explanation

- `whoami` displays the current logged-in username.
- `hostname` displays the system's hostname.

### Lab Output

```text
ubuntu
ip-172-31-10-80
```

---

## 2. Check System Uptime

### Command 1: `uptime`

```bash
uptime
```

This command displays:

- Current system time.
- How long the system has been running.
- Number of logged-in users.
- Load averages for the last 1, 5, and 15 minutes.

### Command 2: `uptime -p`

```bash
uptime -p
```

Displays uptime in a human-readable format.

Example:

```text
up 3 minutes
```

### Command 3: `uptime -s`

```bash
uptime -s
```

Displays the date and time when the system booted.

Lab output:

```text
2026-11-16 11:51:12
```

---

## 3. Check Logged-In Users

### Command

```bash
who
```

### Explanation

The `who` command displays currently logged-in user sessions.

Lab output showed three sessions:

- `seat0` — login screen.
- `:0` — graphical desktop session.
- `pts/0` — SSH terminal session.

---

## 4. Understand Load Average

### Command

```bash
cat /proc/loadavg
```

Example lab output:

```text
0.55 0.50 0.22 1/647 10010
```

### Explanation of the fields

| Field | Meaning |
|---|---|
| First value: `0.55` | 1-minute load average |
| Second value: `0.50` | 5-minute load average |
| Third value: `0.22` | 15-minute load average |
| Fourth value: `1/647` | Running/runnable tasks and total tasks |
| Fifth value: `10010` | Most recently allocated process ID |

**Important:** Linux load average represents tasks that are running or waiting to run, including tasks in uninterruptible sleep (often waiting for I/O). It is not the same as CPU utilization percentage.

---

## 5. Check CPU Count

### Command

```bash
nproc
```

Lab output:

```text
2
```

The system reported two available processing units.

CPU count helps us interpret load averages. For example, a load average around 2 on a two-CPU system indicates approximately two tasks' worth of sustained load.

---

## 6. Generate Temporary CPU Load

### Command

```bash
for i in $(seq "$(nproc)"); do timeout 60 yes > /dev/null & done
```

### Explanation

- `nproc` gets the available CPU count.
- `seq` generates the required number of loop values.
- `yes` continuously produces output.
- `> /dev/null` discards that output.
- `&` runs each command in the background.
- `timeout 60` automatically stops each process after 60 seconds.

Two temporary CPU-load jobs were started because the system had two processing units.

### Verify Background Jobs

```bash
jobs -l
```

Both jobs were running successfully.

---

## 7. Observe Load Average Under CPU Load

After allowing the processes to run, I checked:

```bash
cat /proc/loadavg
uptime
```

### Before CPU Load

```text
0.47 0.48 0.22
```

### During CPU Load

```text
1.79 0.92 0.40
```

### After the Processes Stopped

```text
1.31 0.88 0.41
```

### Observation

The 1-minute load average increased from `0.47` to `1.79` during the CPU-load experiment.

After the processes stopped, the load average began decreasing.

Load averages change gradually because Linux calculates averages over time.

---

## 8. Final Process Verification

### Command

```bash
pgrep -c yes
```

Output:

```text
0
```

This confirmed that no `yes` CPU-load processes remained running.

### Check Human-Readable Uptime

```bash
uptime -p
```

Output:

```text
up 6 minutes
```

---

## 📚 Important Commands Summary

| Command | Purpose |
|---|---|
| `whoami` | Display current username |
| `hostname` | Display system hostname |
| `uptime` | Show uptime and load averages |
| `uptime -p` | Show readable uptime |
| `uptime -s` | Show system boot time |
| `who` | Show logged-in sessions |
| `cat /proc/loadavg` | Read system load averages |
| `nproc` | Display available processing units |
| `jobs -l` | List shell background jobs |
| `pgrep -c yes` | Count running processes named yes |

---

## 🧠 What I Learned

1. How to check system uptime using Linux commands.
2. How to identify the system's last boot time.
3. How to inspect active login sessions.
4. How to read and interpret Linux load averages.
5. How CPU count helps in understanding system load.
6. How to generate temporary CPU load safely using `timeout`.
7. How to verify that background processes have stopped.

---

## 📁 Evidence

Terminal evidence is available in:

```text
evidence/system-uptime-results.txt
```

---

## ✅ Conclusion

In this lab, I successfully checked Linux system uptime, boot time, logged-in sessions, and load averages.

I also generated temporary CPU load using two timeout-controlled processes and observed the increase in system load average.

Finally, I verified that the temporary processes had stopped successfully.

**Status: Practical tasks completed and verified.**
