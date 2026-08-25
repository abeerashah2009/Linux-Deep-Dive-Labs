# Lab 31: Using SCP and SFTP

## Overview

This lab demonstrates secure file transfer between a local Linux system and a remote SSH endpoint using **SCP (Secure Copy Protocol)** and **SFTP (SSH File Transfer Protocol)**.

The lab was performed on an Ubuntu Linux environment using the local machine as both the client and SSH-enabled remote endpoint through `127.0.0.1`.

The following activities were completed:

- Verified SSH, SCP, and SFTP availability.
- Verified that the SSH server was running.
- Configured a dedicated ED25519 SSH key for the lab.
- Established an authenticated SSH connection to localhost.
- Transferred a file from local to remote using SCP.
- Transferred the file from remote back to local using SCP.
- Started an interactive SFTP session.
- Uploaded a file using the SFTP `put` command.
- Downloaded a file using the SFTP `get` command.
- Verified transferred file sizes.
- Verified transferred file contents.
- Generated SHA-256 hashes for original and transferred files.
- Compared hashes to confirm file integrity.
- Performed automated integrity checks.

---

## Objectives

By completing this lab, the following objectives were achieved:

1. Understand SCP and its basic syntax.
2. Use SCP to securely copy files between systems.
3. Understand SFTP and its interactive file-transfer interface.
4. Upload files using the SFTP `put` command.
5. Download files using the SFTP `get` command.
6. Verify file integrity after transfer.
7. Use SHA-256 hashes to confirm that transferred files were not modified.
8. Apply SSH key-based authentication for secure file transfers.

---

## Prerequisites

- Linux terminal access.
- Basic command-line knowledge.
- SSH client.
- SSH server.
- SCP utility.
- SFTP utility.
- Basic knowledge of SSH authentication.
- Basic understanding of file paths and permissions.

---

## Environment

| Item | Value |
|---|---|
| Operating System | Ubuntu Linux |
| Local User | `ubuntu` |
| Hostname | `ip-172-31-10-52` |
| SSH Endpoint | `127.0.0.1` |
| SSH Port | `22` |
| Authentication | ED25519 public key |
| SCP | `/usr/bin/scp` |
| SFTP | `/usr/bin/sftp` |
| SSH | `/usr/bin/ssh` |

---

# Task 1: Securely Copy a File with SCP

## 1.1 SCP Concept

SCP stands for **Secure Copy Protocol**.

SCP uses SSH to securely transfer files between systems.

Basic syntax:

```bash
scp [options] [source] [destination]
```

Common options include:

- `-i` — specify an SSH private key.
- `-r` — recursively copy directories.
- `-C` — enable compression.
- `-P` — specify an SSH port.

For this lab, the dedicated SSH key was specified using `-i`.

---

## 1.2 Verify SCP Availability

The SCP executable was verified with:

```bash
command -v scp
```

Result:

```text
/usr/bin/scp
```

SCP was therefore available and ready for use.

---

## 1.3 SSH Server Verification

The SSH service was checked using:

```bash
sudo systemctl status ssh --no-pager -l
```

The service was reported as:

```text
Active: active (running)
```

The SSH server was listening on port 22.

---

## 1.4 SSH Key Configuration

A dedicated ED25519 key pair was generated for the lab:

```bash
ssh-keygen -t ed25519 \
  -f ~/.ssh/lab31_localhost \
  -N "" \
  -C "lab31-localhost"
```

Generated files:

```text
~/.ssh/lab31_localhost
~/.ssh/lab31_localhost.pub
```

The public key was added to:

```text
~/.ssh/authorized_keys
```

Permissions were secured using:

```bash
chmod 700 ~/.ssh
chmod 600 ~/.ssh/authorized_keys
chmod 600 ~/.ssh/lab31_localhost
chmod 644 ~/.ssh/lab31_localhost.pub
```

The SSH connection was then tested successfully:

```bash
ssh -i ~/.ssh/lab31_localhost \
    -o BatchMode=yes \
    -o StrictHostKeyChecking=yes \
    ubuntu@127.0.0.1 \
    "echo 'SSH localhost connection successful'; whoami; hostname; pwd"
```

Result:

```text
SSH localhost connection successful
ubuntu
ip-172-31-10-52
/home/ubuntu
```

---

## 1.5 Create SCP Sample File

A sample file was created:

```bash
mkdir -p scp-demo

cat > scp-demo/example.txt <<'EOF'
Lab 31 - Secure File Transfer Demonstration
Protocol: SCP
Purpose: Demonstrate secure local-to-remote and remote-to-local file transfer.
Environment: Ubuntu Linux
EOF
```

The source file size was:

```text
163 bytes
```

---

## 1.6 Prepare Remote SCP Directory

A remote test directory was created:

```bash
ssh -i ~/.ssh/lab31_localhost \
    -o BatchMode=yes \
    -o StrictHostKeyChecking=yes \
    ubuntu@127.0.0.1 \
    "mkdir -p /tmp/lab31-scp-remote"
```

Remote directory:

```text
/tmp/lab31-scp-remote/
```

---

## 1.7 SCP Local to Remote

The sample file was transferred from the local system to the remote endpoint:

```bash
scp -i ~/.ssh/lab31_localhost \
    -o BatchMode=yes \
    -o StrictHostKeyChecking=yes \
    scp-demo/example.txt \
    ubuntu@127.0.0.1:/tmp/lab31-scp-remote/
```

Transfer result:

```text
example.txt 100% 163 485.1KB/s 00:00
```

The transfer completed successfully.

---

## 1.8 Verify Remote SCP File

The remote file was verified using SSH:

```bash
ssh -i ~/.ssh/lab31_localhost \
    -o BatchMode=yes \
    -o StrictHostKeyChecking=yes \
    ubuntu@127.0.0.1 \
    "ls -lh /tmp/lab31-scp-remote/example.txt"
```

Result:

```text
-rw-rw-r-- 1 ubuntu ubuntu 163 ... /tmp/lab31-scp-remote/example.txt
```

The remote file contained the expected data.

---

## 1.9 SCP Remote to Local

A download directory was created:

```bash
mkdir -p scp-demo/downloaded
```

The remote file was copied back to the local system:

```bash
scp -i ~/.ssh/lab31_localhost \
    -o BatchMode=yes \
    -o StrictHostKeyChecking=yes \
    ubuntu@127.0.0.1:/tmp/lab31-scp-remote/example.txt \
    scp-demo/downloaded/
```

Transfer result:

```text
example.txt 100% 163 ... 00:00
```

The download completed successfully.

---

## 1.10 Verify Downloaded SCP File

The downloaded file was checked:

```bash
ls -lh scp-demo/downloaded/example.txt
```

Result:

```text
-rw-rw-r-- 1 ubuntu ubuntu 163 ... scp-demo/downloaded/example.txt
```

The file content matched the original source file.

---

# Task 2: Upload and Download Files with SFTP

## 2.1 SFTP Concept

SFTP stands for **SSH File Transfer Protocol**.

Unlike traditional FTP, SFTP operates through SSH and provides encrypted authentication and file transfer.

The SFTP client provides an interactive interface with commands such as:

- `pwd` — display remote working directory.
- `ls` — list remote files.
- `put` — upload a local file.
- `get` — download a remote file.
- `exit` — close the SFTP session.

---

## 2.2 Verify SFTP Availability

The SFTP executable was verified using:

```bash
command -v sftp
```

Result:

```text
/usr/bin/sftp
```

---

## 2.3 Create SFTP Sample File

A separate SFTP test file was created:

```bash
mkdir -p sftp-demo/downloaded

cat > sftp-demo/sftp-example.txt <<'EOF'
Lab 31 - SFTP Secure File Transfer Demonstration
Protocol: SFTP
Purpose: Demonstrate secure file upload and download operations.
Environment: Ubuntu Linux
EOF
```

Source file size:

```text
155 bytes
```

---

## 2.4 Prepare Remote SFTP Directory

The remote SFTP directory was created:

```bash
ssh -i ~/.ssh/lab31_localhost \
    -o BatchMode=yes \
    -o StrictHostKeyChecking=yes \
    ubuntu@127.0.0.1 \
    "mkdir -p /tmp/lab31-sftp-remote"
```

Remote directory:

```text
/tmp/lab31-sftp-remote/
```

---

## 2.5 Start SFTP Session

An authenticated SFTP session was started using:

```bash
sftp -i ~/.ssh/lab31_localhost \
     -o BatchMode=yes \
     -o StrictHostKeyChecking=yes \
     ubuntu@127.0.0.1
```

The connection was successful:

```text
Connected to 127.0.0.1.
sftp>
```

The remote working directory was verified:

```text
Remote working directory: /home/ubuntu
```

---

## 2.6 Upload File Using SFTP `put`

Inside the SFTP session, the file was uploaded:

```text
put /home/ubuntu/Linux-Deep-Dive-Labs/Lab-31-Using-SCP-and-SFTP/sftp-demo/sftp-example.txt /tmp/lab31-sftp-remote/
```

Transfer result:

```text
sftp-example.txt 100% 155 548.1KB/s 00:00
```

The remote file was then verified:

```text
-rw-rw-r-- ubuntu ubuntu 155B ... /tmp/lab31-sftp-remote/sftp-example.txt
```

---

## 2.7 Download File Using SFTP `get`

The same file was downloaded back to the local system:

```text
get /tmp/lab31-sftp-remote/sftp-example.txt /home/ubuntu/Linux-Deep-Dive-Labs/Lab-31-Using-SCP-and-SFTP/sftp-demo/downloaded/
```

Transfer result:

```text
sftp-example.txt 100% 155 300.1KB/s 00:00
```

The SFTP download completed successfully.

---

## 2.8 Exit SFTP

The SFTP session was terminated with:

```text
exit
```

---

## 2.9 Verify SFTP Download

The downloaded file was verified:

```bash
ls -lh sftp-demo/downloaded/sftp-example.txt
```

Result:

```text
-rw-rw-r-- 1 ubuntu ubuntu 155 ... sftp-demo/downloaded/sftp-example.txt
```

The source and downloaded files were both verified as:

```text
155 bytes
```

The downloaded content matched the original file.

---

# Task 3: Verify File Integrity

## 3.1 SHA-256 Concept

SHA-256 is a cryptographic hash function that produces a fixed-length 256-bit digest.

A SHA-256 hash can be used to compare two files.

If the original file and transferred file produce the same SHA-256 hash, the files have matching content.

The command used was:

```bash
sha256sum filename
```

---

## 3.2 SCP SHA-256 Verification

Original SCP file:

```text
scp-demo/example.txt
```

SHA-256:

```text
673c0132360d2974495630e0827c61150a8cbafae367d968ae3fd26b0e77e09c
```

Downloaded SCP file:

```text
scp-demo/downloaded/example.txt
```

SHA-256:

```text
673c0132360d2974495630e0827c61150a8cbafae367d968ae3fd26b0e77e09c
```

Both hashes matched.

Result:

```text
SCP SHA-256 verification: PASS
```

---

## 3.3 SFTP SHA-256 Verification

Original SFTP file:

```text
sftp-demo/sftp-example.txt
```

SHA-256:

```text
63571bae4ec2e65c273513bbaa558cfa3af9f36e4c46e2a1a9d13b3f1e1c8b1e
```

Downloaded SFTP file:

```text
sftp-demo/downloaded/sftp-example.txt
```

SHA-256:

```text
63571bae4ec2e65c273513bbaa558cfa3af9f36e4c46e2a1a9d13b3f1e1c8b1e
```

Both hashes matched.

Result:

```text
SFTP SHA-256 verification: PASS
```

---

## 3.4 Automated Integrity Verification

The files were also compared using:

```bash
cmp -s scp-demo/example.txt scp-demo/downloaded/example.txt
```

Result:

```text
SCP integrity: PASS
```

The SFTP files were compared using:

```bash
cmp -s sftp-demo/sftp-example.txt sftp-demo/downloaded/sftp-example.txt
```

Result:

```text
SFTP integrity: PASS
```

Both transfer methods passed automated content comparison.

---

# Final Verification

The complete lab demonstrated:

```text
                    SSH
                     │
          ┌──────────┴──────────┐
          │                     │
         SCP                   SFTP
          │                     │
     ┌────┴────┐           ┌────┴────┐
     │         │           │         │
   Upload   Download      put       get
     │         │           │         │
     └────┬────┘           └────┬────┘
          │                     │
          └──────────┬──────────┘
                     │
                SHA-256
                Verification
                     │
                  PASS
```

## Verification Results

| Test | Result |
|---|---|
| SSH client available | PASS |
| SCP available | PASS |
| SFTP available | PASS |
| SSH server running | PASS |
| SSH public-key authentication | PASS |
| SCP local → remote | PASS |
| SCP remote → local | PASS |
| SFTP connection | PASS |
| SFTP `put` | PASS |
| SFTP `get` | PASS |
| SCP file size | PASS |
| SFTP file size | PASS |
| SCP content comparison | PASS |
| SFTP content comparison | PASS |
| SCP SHA-256 verification | PASS |
| SFTP SHA-256 verification | PASS |

---

# Security Considerations

Secure file transfer should be performed using authenticated SSH connections.

Important practices include:

- Use SSH keys instead of passwords where appropriate.
- Protect private keys with restrictive permissions.
- Keep `~/.ssh` permissions secure.
- Keep `authorized_keys` permissions restrictive.
- Verify SSH host keys.
- Avoid disabling host-key verification unnecessarily.
- Use encrypted protocols such as SCP and SFTP instead of unencrypted FTP.
- Verify file integrity after important transfers.
- Use SHA-256 or another suitable cryptographic hash for integrity checking.

The lab used:

```bash
-o StrictHostKeyChecking=yes
```

for transfer operations after the localhost host key had been explicitly established.

---

# Troubleshooting

## Host Key Verification Failed

If SSH reports:

```text
Host key verification failed.
```

inspect the known hosts entries:

```bash
ssh-keygen -F 127.0.0.1
```

For a new lab environment, a host key can be established using an appropriate first-connection process.

---

## Permission Denied (publickey)

If SSH reports:

```text
Permission denied (publickey).
```

verify:

```bash
ls -la ~/.ssh
```

Check that the public key is present in:

```text
~/.ssh/authorized_keys
```

and that permissions are appropriate:

```bash
chmod 700 ~/.ssh
chmod 600 ~/.ssh/authorized_keys
```

---

## Verify SSH Service

Check the SSH service:

```bash
sudo systemctl status ssh
```

The expected state is:

```text
Active: active (running)
```

---

# Lab Directory Structure

```text
Lab-31-Using-SCP-and-SFTP/
├── README.md
├── scp-demo/
│   ├── example.txt
│   └── downloaded/
│       └── example.txt
└── sftp-demo/
    ├── sftp-example.txt
    └── downloaded/
        └── sftp-example.txt
```

Temporary remote test directories were created outside the Git repository:

```text
/tmp/lab31-scp-remote/
/tmp/lab31-sftp-remote/
```

SSH private keys were also kept outside the repository and were **not committed to Git**.

---

# Conclusion

This lab successfully demonstrated secure file transfer using both SCP and SFTP.

SCP was used to transfer files in both directions:

```text
Local → Remote
Remote → Local
```

SFTP was used to perform interactive file transfers:

```text
put
get
```

File sizes and contents were verified after transfer.

Finally, SHA-256 hashes were generated for the original and transferred files. The hashes matched for both SCP and SFTP transfers.

Final results:

```text
SCP integrity: PASS
SFTP integrity: PASS
SCP SHA-256 verification: PASS
SFTP SHA-256 verification: PASS
```

The lab therefore successfully achieved all stated objectives and demonstrated practical secure file-transfer and integrity-verification skills.
