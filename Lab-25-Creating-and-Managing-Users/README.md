# Lab 25: Creating and Managing Users

## Lab Objective

The objective of this lab is to understand how Linux user accounts are created, managed, and deleted. In this lab, we will use commands such as `useradd`, `passwd`, and `userdel` to perform basic user administration tasks.

## Prerequisites

* Basic knowledge of Linux terminal commands.
* Access to a Linux-based system.
* Sudo or root privileges.
* Basic understanding of Linux user accounts.

---

# Task 1: Create a New User

## Step 1.1: Create the User

The `useradd` command is used to create a new user account.

### Command

```bash
sudo useradd newuser
```

### Explanation

* `sudo` gives administrator privileges.
* `useradd` creates a new Linux user account.
* `newuser` is the username being created.

After running the command, the user account is added to the system.

## Step 1.2: Verify the User

To verify that the user was successfully created, use:

```bash
id newuser
```

### Example Output

```text
uid=1001(newuser) gid=1001(newuser) groups=1001(newuser)
```

The `id` command displays the user's UID, primary group ID, and group memberships.

Another way to verify the account is:

```bash
grep newuser /etc/passwd
```

The `/etc/passwd` file contains information about local user accounts.

---

# Task 2: Set a Password for the User

## Step 2.1: Assign a Password

The `passwd` command is used to create or change a user's password.

### Command

```bash
sudo passwd newuser
```

The system will ask for the new password and then ask you to enter it again for confirmation.

### Example

```text
New password:
Retype new password:
passwd: password updated successfully
```

### Explanation

The `passwd` command updates the authentication information for the specified user.

## Step 2.2: Verify the Password Status

The password status can be checked using:

```bash
sudo passwd -S newuser
```

This command displays information about the user's password status.

---

# Task 3: Delete the User

## Step 3.1: Remove the User Account

The `userdel` command is used to remove a user account from Linux.

### Command

```bash
sudo userdel newuser
```

This removes the user account from the system.

## Step 3.2: Verify User Deletion

To confirm that the user has been removed, run:

```bash
id newuser
```

Expected result:

```text
id: ‘newuser’: no such user
```

You can also check the `/etc/passwd` file:

```bash
grep newuser /etc/passwd
```

If there is no output, the user account has been successfully removed.

---

# Task 4: Delete a User Along With the Home Directory

Linux also provides the `-r` option with `userdel`.

### Command

```bash
sudo userdel -r newuser
```

The `-r` option removes the user's account along with their home directory and associated files.

### Important Note

The `-r` option should be used carefully because files stored in the user's home directory will also be deleted.

---

# Commands Used in This Lab

```bash
sudo useradd newuser
```

```bash
id newuser
```

```bash
grep newuser /etc/passwd
```

```bash
sudo passwd newuser
```

```bash
sudo passwd -S newuser
```

```bash
sudo userdel newuser
```

```bash
id newuser
```

```bash
sudo userdel -r newuser
```

---

# Key Concepts Learned

### `useradd`

Creates a new user account.

### `passwd`

Creates or changes a user's password.

### `id`

Displays information about a user, including UID and group information.

### `/etc/passwd`

Contains basic information about local Linux user accounts.

### `userdel`

Removes a user account.

### `userdel -r`

Removes a user account and its home directory.

---

# Practical Application

User management is an important responsibility of a Linux system administrator. When a new employee joins an organization, an administrator can create a user account and assign appropriate permissions. When an employee leaves, their account can be disabled or removed to maintain system security.

Proper user management helps organizations:

* Control access to systems and resources.
* Protect sensitive information.
* Maintain organized user accounts.
* Improve system security.
* Prevent unauthorized access.

---

# Conclusion

In this lab, we learned the basic process of managing Linux user accounts. We created a new user using `useradd`, assigned a password using `passwd`, verified the account using `id`, and removed the user using `userdel`.

We also learned about the `-r` option, which can be used to remove a user together with their home directory.

These commands provide essential skills for Linux system administration and are important for managing users and controlling access to Linux systems.
