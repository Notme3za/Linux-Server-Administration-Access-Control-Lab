# Linux Server Administration & Access Control Lab

## Overview

This project documents a hands-on Linux administration lab using Linux Mint 22.3 in Oracle VirtualBox.

The goal was to practice managing user access, securing SSH remote administration, configuring firewall rules, and verifying Linux file permissions.

## Lab Environment

- **Operating system:** Linux Mint 22.3
- **Virtualization:** Oracle VirtualBox
- **Network configuration:** Bridged Adapter
- **SSH client:** Windows OpenSSH client
- **Firewall:** UFW (Uncomplicated Firewall)

## Objectives

- Create a standard Linux user for remote administration.
- Configure SSH access restrictions.
- Disable SSH login for the root account.
- Restrict incoming SSH connections using UFW.
- Test Linux filesystem permissions between different users.
- Verify the results through terminal commands and connection tests.

## What I Implemented

### 1. User Management

Created a dedicated account named `labuser` for remote access.

Verified its user ID and group membership to check that it was not a member of the standard administrative groups.

### 2. SSH Access Control

Installed the OpenSSH server and configured the following settings in `/etc/ssh/sshd_config.d/99-lab-access.conf`:

```text
PermitRootLogin no
AllowUsers labuser
```

Validated the SSH configuration and checked the effective settings.

### 3. Remote Administration

Connected from the Windows host to the Linux Mint virtual machine over SSH.

```bash
ssh labuser@192.168.100.55
```

Verified the remote account and hostname after logging in.

### 4. Firewall Configuration

Configured UFW to deny incoming connections by default and allow outgoing connections.

Added a rule allowing TCP port 22 only from the local IPv4 subnet:

```bash
sudo ufw allow from 192.168.100.0/24 to any port 22 proto tcp
```

Verified the firewall rule using `sudo ufw status numbered`.

### 5. Filesystem Permissions

Created a temporary private directory and file owned by the main account, `mhmd`.

Applied restrictive permissions:

- Directory: `700`
- File: `600`

Tested access while logged in as `labuser`. The attempt to read the private file returned `Permission denied`, confirming that the directory permissions prevented access.

The temporary test files were removed after verification.

## Verification Results

| Test | Result |
|---|---|
| SSH login as `labuser` from Windows | Successful |
| SSH login as `mhmd` | Rejected |
| SSH login as `root` | Rejected |
| Read private file as `labuser` | Denied |
| SSH configuration syntax check | Passed |
| UFW status and SSH rule | Verified |
| SSH listener on port 22 | Verified |

## What I Learned

This lab helped me understand the relationship between Linux user accounts, SSH authentication, firewall rules, and filesystem permissions.

It also demonstrated that successfully connecting to a server does not automatically grant access to every file or administrative function on that machine.

## Limitations

- This was a lab environment running inside VirtualBox, not a production server.
- Password authentication remained enabled for the permitted SSH account.
- IPv6 firewall behavior was not fully verified.
- Configuration behavior after a reboot was not tested.

## Conclusion

This project provided practical experience with Linux server administration and access control. It was an opportunity to configure security restrictions, test them from a separate machine, and observe how Linux enforces access permissions.