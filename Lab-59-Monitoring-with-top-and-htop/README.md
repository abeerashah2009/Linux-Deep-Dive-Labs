# Lab 59: Monitoring with `top` and `htop`

## 1. Lab Overview

This lab explains how to monitor Linux system resources and running processes using `top` and `htop`.

System monitoring helps administrators understand CPU usage, memory consumption, system load, and process activity.

**Environment:**
- Operating System: Ubuntu 24.04 LTS
- Monitoring tools: `top` and `htop`
- Package version: htop 3.3.0
- Instance: Ubuntu AWS EC2 lab environment

## 2. Objectives

- Understand the purpose of `top` and `htop`.
- Install and verify `htop`.
- Monitor running processes and system resources.
- Compare the `top` and `htop` interfaces.
- Sort processes by CPU usage.
- Sort processes by memory usage.
- Create test processes and safely terminate them.

## 3. Understanding System Monitoring

System monitoring means observing the performance and resource usage of a Linux computer.

Important resources include:

| Resource | Meaning |
|---|---|
| CPU | Processes the system's instructions |
| RAM | Temporary memory used by running programs |
| Swap | Disk space used as additional virtual memory |
| Load Average | Average system workload over time |
| Process | A running instance of a program |

Monitoring helps identify processes that consume too much CPU or memory.

## 4. Understanding the `top` Command

The `top` command displays a live view of system performance and running processes.

It provides information such as:
- System uptime
- Load average
- Number of running and sleeping tasks
- CPU usage
- Memory usage
- Process IDs (PIDs)
- Process owners
- CPU and memory consumption by processes

**Basic command:**

```bash
top
```

For this lab, batch mode was used because it does not require an interactive full-screen terminal.

```bash
top -b -n1
```

Explanation:
- `-b`: Runs `top` in batch mode.
- `-n1`: Takes one monitoring snapshot.

To display only the first 15 lines:

```bash
top -b -n1 | head -15
```

## 5. Installing and Verifying `htop`

`htop` is an interactive process-monitoring tool with a visual interface.

Update the package information:

```bash
sudo apt update
```

Install `htop`:

```bash
sudo apt install -y htop
```

Verify the executable location and version:

```bash
command -v htop
htop --version
```

**Actual verification:**

```text
/usr/bin/htop
htop 3.3.0
```

The package was already installed in this environment, so APT confirmed that the installed version was the newest available version.

## 6. Running `htop`

Start the interactive interface:

```bash
htop
```

The interface displays:
- CPU usage bars
- Memory usage
- Swap usage
- Running processes
- Process IDs
- Resource usage for individual processes

Press `q` to exit.

Useful interactive controls include:
- Arrow keys: Move through the process list.
- F3: Search for a process.
- F9: Send a signal to a selected process.

## 7. Creating Test Processes

To observe CPU and memory consumption, two temporary test processes were created.

### CPU test process

```bash
yes > /dev/null &
```

The `yes` command continuously produces output. Redirecting its output to `/dev/null` prevents it from filling the terminal.

The `&` symbol runs the process in the background.

### Memory test process

```bash
python3 -c 'b = bytearray(300*1024*1024); import time; time.sleep(600)' &
```

This Python command allocates approximately 300 MiB of memory and remains running temporarily.

The process IDs were stored in shell variables:

```bash
CPU_PID=$!
```

```bash
MEM_PID=$!
```

In the practical session, the recorded process IDs were:
- CPU process: `12863`
- Memory process: `13384`

## 8. Monitoring CPU and Memory Usage

Run:

```bash
top -b -n1 | head -15
```

This displays a snapshot of system activity.

To inspect processes sorted by CPU usage:

```bash
ps aux --sort=-%cpu | head -5
```

During the lab, the `yes` process reached approximately 95.6% CPU usage in one snapshot.

To inspect processes sorted by memory usage:

```bash
ps aux --sort=-%mem | head
```

The temporary Python process used approximately:
- 317 MB resident memory (RSS)
- 8.0% to 8.1% memory

These values are observations from the lab environment and may change between monitoring snapshots.

## 9. Comparing `top` and `htop`

| Feature | top | htop |
|---|---|---|
| Interface | Text-based | Interactive visual interface |
| CPU monitoring | Yes | Yes |
| Memory monitoring | Yes | Yes |
| Process sorting | Yes | Yes |
| Process search | Available through commands | Interactive search |
| Batch mode | Yes | Primarily interactive |
| Ease of navigation | Keyboard controls | More visual navigation |

Both tools help administrators monitor processes and system resources.

## 10. Comparing Memory Information

Use `top` to view memory information:

```bash
top -b -n1 | head -5
```

Use `free` for a memory summary:

```bash
free -m
```

The `-m` option displays memory values in megabytes.

**Observed values during this lab:**

| Memory field | Approximate value |
|---|---:|
| Total memory | 3832 MB |
| Used memory | 1705 MB |
| Free memory | 962 MB |
| Buff/cache | 1421 MB |
| Available memory | 2126 MB |

Small differences between monitoring commands are normal because system resource usage changes over time.

## 11. Sorting Processes by CPU Usage

Use:

```bash
top -b -n1 -o %CPU | head -15
```

This displays processes ordered by CPU usage.

To display the process-list section:

```bash
top -b -n1 -o %CPU | sed -n '7,10p'
```

Another method is:

```bash
ps aux --sort=-%cpu | head
```

The `-` sign sorts values in descending order, placing higher CPU-consuming processes first.

**Lab observation:** The `yes` process appeared at the top of the CPU-sorted output.

## 12. Sorting Processes by Memory Usage

Use:

```bash
top -b -n1 -o %MEM | head -15
```

Or:

```bash
ps aux --sort=-%mem | head
```

This helps identify processes consuming the highest percentage of RAM.

**Lab observation:** The Python memory test process appeared near the top of the memory-sorted list.

## 13. Understanding Important Process Fields

| Field | Meaning |
|---|---|
| PID | Unique process identification number |
| USER | User who owns the process |
| PR | Process priority |
| NI | Nice value |
| VIRT | Total virtual memory |
| RES | Resident physical memory |
| SHR | Shared memory |
| S | Process state |
| %CPU | CPU usage percentage |
| %MEM | Memory usage percentage |
| TIME+ | Total accumulated CPU time |
| COMMAND | Program or command name |

## 14. Cleaning Up Test Processes

Temporary processes should be stopped after monitoring.

The process IDs were stored in variables, so both processes could be terminated with:

```bash
kill "$CPU_PID" "$MEM_PID"
```

Wait briefly:

```bash
sleep 1
```

Verify that they have stopped:

```bash
ps -p "$CPU_PID","$MEM_PID"
```

The output contained only the header, confirming that both test processes had exited.

## 15. Troubleshooting Notes

**Problem: `htop: command not found`**

Install it using:

```bash
sudo apt update
sudo apt install -y htop
```

**Problem: `top` output is difficult to read in a terminal session**

Use batch mode:

```bash
top -b -n1
```

**Problem: A test process continues running**

Check its PID:

```bash
ps aux | grep '[y]es'
```

Terminate the correct test process using its PID:

```bash
kill PID
```

Replace `PID` with the actual process ID.

## 16. Interview Questions and Answers

**Q1. What is the purpose of the `top` command?**

It displays running processes and real-time system resource information, including CPU and memory usage.

**Q2. What is `htop`?**

It is an interactive process-monitoring tool that provides a visual display of system resources and running processes.

**Q3. What is the difference between `top` and `htop`?**

`top` provides a text-based monitoring interface, while `htop` provides a more visual and interactive interface.

**Q4. What does PID mean?**

PID stands for Process ID. It is a unique number assigned to a running process.

**Q5. How can we sort processes by CPU usage?**

```bash
ps aux --sort=-%cpu
```

**Q6. How can we sort processes by memory usage?**

```bash
ps aux --sort=-%mem
```

**Q7. What does the `free -m` command do?**

It displays memory usage information in megabytes.

**Q8. What is the purpose of `kill`?**

It sends a signal to a process. The default signal requests that the process terminate.

**Q9. What does batch mode mean in `top`?**

Batch mode displays output without opening the interactive interface. It is useful for scripts and evidence collection.

**Q10. Why is system monitoring important?**

It helps administrators identify resource-intensive processes, investigate performance issues, and maintain system stability.

## 17. Evidence

Terminal evidence is available in:

```text
evidence/top-htop-results.txt
```

The evidence records:
- APT update and `htop` verification
- CPU and memory test processes
- `top` monitoring output
- `htop` interface observation
- Memory comparison
- CPU and memory sorting
- Process cleanup and verification

## 18. Conclusion

In this lab, I learned how to monitor Linux system performance using `top` and `htop`.

I installed and verified `htop`, observed CPU and memory usage, compared monitoring tools, sorted processes by resource consumption, and safely terminated temporary test processes.

These skills are useful for Linux administration, troubleshooting, and DevOps operations.
