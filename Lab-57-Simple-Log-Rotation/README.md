# Lab 57 — Simple Log Rotation

## Overview

This lab demonstrates how to inspect, configure, test, and manually execute log rotation on Ubuntu 24.04 using **Logrotate**.

Log rotation helps system administrators manage log files by creating new log files, retaining older logs, and compressing archived logs. This prevents log files from consuming excessive disk space.

## Learning Objectives

- Understand the basics of log rotation.
- Inspect the global Logrotate configuration.
- Understand application-specific Logrotate configurations.
- Create and modify a custom Logrotate configuration.
- Perform a dry run before making changes.
- Manually force log rotation.
- Verify delayed compression.
- Safely clean up temporary test files.

## Environment

| Component | Details |
|---|---|
| Operating System | Ubuntu 24.04 LTS |
| Tool | Logrotate |
| Logrotate Version | 3.21.0 |
| Environment | AWS EC2 |
| Shell | Bash |

## Task 1 — Install and Inspect Logrotate

Verified the installed version:

```bash
logrotate --version | head -1
```

Inspected the global configuration:

```bash
cat /etc/logrotate.conf
```

Important directives:

| Directive | Purpose |
|---|---|
| `weekly` | Rotate logs weekly |
| `rotate 4` | Retain four old rotations |
| `su root adm` | Use root user and adm group |
| `create` | Create a new empty log file |
| `compress` | Compress rotated logs when enabled |
| `include` | Load additional configuration files |

The global configuration was inspected but not modified.

## Task 2 — Inspect Application Configuration

Inspected:

```bash
cat /etc/logrotate.d/rsyslog
```

Reviewed directives including:

- `rotate`
- `weekly`
- `missingok`
- `notifempty`
- `compress`
- `delaycompress`
- `sharedscripts`
- `postrotate`

The system rsyslog configuration was left unchanged.

### Create a Private Test Log

Created a test log containing 1,000 lines:

```bash
mkdir -p ~/lab
sudo mkdir -p /var/log/lab
seq 1 1000 | sudo tee /var/log/lab/app.log > /dev/null
```

Verified:

```bash
wc -l /var/log/lab/app.log
```

### Create a Custom Configuration

Created `~/lab/app.logrotate`:

```text
/var/log/lab/app.log {
    su root root
    daily
    rotate 3
    missingok
    notifempty
    compress
    delaycompress
    create 640 root root
}
```

Updated the retention count:

```bash
sed -i 's/rotate 3/rotate 2/' ~/lab/app.logrotate
```

Verified the setting:

```bash
grep rotate ~/lab/app.logrotate
```

Final retention setting: `rotate 2`.

### Configuration Security

Set secure permissions and ownership:

```bash
chmod 644 ~/lab/app.logrotate
sudo chown root:root ~/lab/app.logrotate
```

Verified that the configuration was owned by root and was not writable by group or others.

## Task 3 — Dry Run and Manual Rotation

### Dry Run

Executed:

```bash
sudo logrotate -d -f -s ~/lab/state ~/lab/app.logrotate
```

Verified:

- Logrotate accepted the configuration.
- One log was handled.
- The log needed rotation.
- The configuration passed ownership and permission checks.

Debug mode did not modify the log files.

### First Manual Rotation

Executed:

```bash
sudo logrotate -f -s ~/lab/state ~/lab/app.logrotate
```

Verified the generated files:

- `app.log` — new empty log.
- `app.log.1` — previous log containing 1,000 lines.

### Second Manual Rotation

Added another 500 lines:

```bash
seq 1 500 | sudo tee /var/log/lab/app.log > /dev/null
```

Verified the line count:

```bash
sudo wc -l /var/log/lab/app.log
```

Executed the second rotation:

```bash
sudo logrotate -f -s ~/lab/state ~/lab/app.logrotate
```

Expected files were verified:

| File | Description |
|---|---|
| `app.log` | New empty log |
| `app.log.1` | Previous 500-line log |
| `app.log.2.gz` | Compressed original 1,000-line log |

### Compression Verification

Executed:

```bash
sudo zcat /var/log/lab/app.log.2.gz | tail -2
```

Verified output:

```text
999
1000
```

This confirmed that the original log data remained readable after compression.

## Cleanup

Removed the temporary test files and directories:

```bash
sudo rm -rf /var/log/lab ~/lab
```

Verified that both directories no longer existed.

No real system log files or system Logrotate configuration files were modified.

## Key Concepts Learned

1. Log rotation controls the growth of log files.
2. Retention settings determine how many old logs are kept.
3. Compression reduces the storage size of archived logs.
4. `delaycompress` postpones compression until the next rotation.
5. Dry runs help identify configuration problems safely.
6. File ownership and permissions are important for Logrotate security.
7. Manual rotation can be triggered with the `-f` option.

## Evidence

Practical terminal results are documented in:

`evidence/logrotate-results.txt`

## Conclusion

Successfully completed the Simple Log Rotation lab on Ubuntu 24.04.

Inspected Logrotate configurations, created a private test environment, changed retention settings, resolved configuration security requirements, performed a dry run, executed two manual rotations, verified compressed log contents, and cleaned up temporary files.

**Lab 57 — Simple Log Rotation: Practical tasks completed and verified.**
