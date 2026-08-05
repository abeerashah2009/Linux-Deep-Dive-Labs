# Lab 30: SSH Basics

## Objective

The objective of this lab is to understand the basics of SSH (Secure Shell) and learn how to securely connect to a remote Linux server.

## What is SSH?

SSH (Secure Shell) is a network protocol used to securely connect to and manage remote computers over a network.

SSH provides encrypted communication between the local computer and the remote server.

## Common SSH Command

The basic syntax for connecting to a remote Linux server is:

```bash
ssh username@server_ip
```

Example:

```bash
ssh ubuntu@192.168.1.100
```

## Important SSH Commands

### 1. Connect to a Remote Server

```bash
ssh username@server_ip
```

This connects to the remote server using the specified username and IP address.

### 2. Connect Using a Specific SSH Key

```bash
ssh -i key.pem username@server_ip
```

The `-i` option specifies the private key used for authentication.

### 3. Specify a Different SSH Port

```bash
ssh -p 2222 username@server_ip
```

The `-p` option allows us to connect using a port other than the default SSH port (22).

### 4. Check SSH Version

```bash
ssh -V
```

This displays the installed SSH client version.

## SSH Default Port

SSH normally uses:

```text
Port: 22
```

## SSH Authentication

SSH commonly supports two authentication methods:

1. Password authentication
2. SSH key-based authentication

SSH key-based authentication is commonly preferred because it is more secure and convenient for server administration.

## SSH Key Files

Common SSH key files include:

```text
~/.ssh/id_rsa
~/.ssh/id_ed25519
```

The private key must be kept secure and should never be shared publicly.

## Useful SSH Files and Directories

SSH configuration and keys are commonly stored inside:

```bash
~/.ssh/
```

To view the directory:

```bash
ls -la ~/.ssh/
```

## Useful SSH Commands Summary

| Command                  | Purpose                       |
| ------------------------ | ----------------------------- |
| `ssh user@IP`            | Connect to a remote server    |
| `ssh -i key.pem user@IP` | Connect using an SSH key      |
| `ssh -p PORT user@IP`    | Connect using a specific port |
| `ssh -V`                 | Check SSH version             |
| `ls -la ~/.ssh/`         | View SSH files                |

## Conclusion

In this lab, I learned the basics of SSH, including how to connect to remote Linux servers, use SSH keys, specify different ports, and identify important SSH files and directories.
