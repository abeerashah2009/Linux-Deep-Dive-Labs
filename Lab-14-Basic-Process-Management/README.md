# Lab 14 - Basic Process Management

## Objectives
- Learn to list running processes.
- Monitor processes using top.
- Kill a process using PID.
- Kill a process using its name.

## Commands Used

### List processes

```bash
ps aux
```

### Monitor processes

```bash
top
```

### Create a test process

```bash
sleep 300 &
```

### Find process

```bash
ps aux | grep sleep
```

### Kill using PID

```bash
kill PID
```

### Kill using process name

```bash
pkill -x sleep
```

## Result

Successfully learned basic Linux process management.
