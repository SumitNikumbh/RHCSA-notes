# Proof of Concept (PoC) Document

**Topic:** Managing Users and Groups  
**Chapter:** RH124 v9 – Chapter 6  
**Prepared by:** Sumit Nikumbh  
**Date:** 08/07/2025  

---

## Executive Summary

This Proof of Concept demonstrates how to manage users and groups on a Red Hat Enterprise Linux 9 (RHEL 9) system. It includes commands for creating, modifying, and deleting users and groups. It also covers password management, default settings, and user environment configuration.

---

## Objective

- Learn to create, modify, and delete users and groups.  
- Understand user-related configuration files and home directory structure.  
- Apply password policies and default group behaviors.

---

## Scope

- User and group management commands.  
- Password and account configuration.  
- Understanding UID/GID and related configuration files.

---

## Requirements

- **Operating System:** RHEL 9  
- **User Privileges:** Root access  
- **Tools Used:** `useradd`, `usermod`, `userdel`, `groupadd`, `passwd`, `id`, `su`, etc.

---

## Methodology / Approach

### Types of Users

- **Root User:** Superuser with UID 0; full administrative control.  
- **Regular User:** Human-created user (UID 1000+); used for personal tasks.  
- **System User:** Non-login service accounts (UID 1–999); used by system processes.

### Important Configuration Files

| File Path          | Purpose                         |
|--------------------|----------------------------------|
| `/etc/passwd`      | User information                |
| `/etc/group`       | Group information               |
| `/etc/shadow`      | Encrypted user password info    |
| `/etc/gshadow`     | Group password info             |

---

## User Management Commands

### 1. Create User

```bash
useradd <username>
passwd <username>
Creates a user and sets their password.

2. Switch User
bash
su - <username>
su - root
Switch from one user to another. Root access requires password from regular user.

3. View User Info
bash
id <username>
Shows UID, GID, and group membership.

4. View Logged-in Users
bash
who
Displays who is currently logged in.

5. Show Current User
bash
whoami
Displays current username.

6. Delete User
bash
userdel <username>
userdel -r <username>
userdel deletes user but keeps home directory.
userdel -r deletes user and home directory.

To manually remove the directory:
bash
rm -rf /home/<username>

7. Modify User with usermod
Option	Description:
-u	Change UID: usermod -u <newUID> <username>
-l	Change login name: usermod -l <newname> <oldname>
-g	Change primary group: usermod -g <group> <username>
-d	Change home directory: usermod -m -d <newpath> <username>
-s	Change login shell: usermod -s /sbin/nologin <username>
-L	Lock user: usermod -L <username>
-U	Unlock user: usermod -U <username>
-c	Change comment: usermod -c "<comment>" <username>
-aG	Add to secondary group(s): usermod -aG <group> <username>

Types of Groups
Primary Group: One per user. Assigned to new files created by the user.

Secondary Group: Users can belong to multiple groups. Used for shared access and permissions.

Group Management Commands
1. Create Group
bash
groupadd <groupname>
groupadd -g <GID> <groupname>
Create a group, optionally specifying GID.

2. Assign Group Password
bash
gpasswd <groupname>

3. Remove User from Group
bash
gpasswd -d <username> <groupname>

4. View Groups of a User
bash
groups <username>
Password Management
passwd Command Options
Command	Purpose
passwd -e <username>	    Force user to change password at next login
passwd -M <days> <username>	Set maximum days before password expires
passwd -x <days> <username>	Set max valid days for password
passwd -n <days> <username>	Set min days between password changes
passwd -w <days> <username>	Set warning period before password expires

Using getent for Account Info
getent passwd — list all users

getent passwd <username> — show a specific users entry

getent group — list all groups

getent group <groupname> — show specific group entry

getent is more powerful than cat /etc/passwd as it shows both local and remote users if configured.

Results / Findings
Successfully created and managed users and groups

Applied user policies and verified configurations

Challenges Faced
Root privileges required for many operations

Conflicts when re-creating users with existing home directories

Conclusion
This PoC successfully demonstrates how to manage users and groups in RHEL 9. It provides essential knowledge for Linux system administration related to access control and user environment management.

Appendix - Summary of Useful Commands
bash
Copy code
# User Management
useradd, usermod, userdel
passwd, id, su, whoami, groups

# Group Management
groupadd, groupdel, gpasswd

# Information Retrieval
getent passwd, getent group
cat /etc/passwd, cat /etc/group, cat /etc/shadow