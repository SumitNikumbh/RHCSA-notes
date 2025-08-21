# 📄 PoC: Chapter 7 – Controlling Access to Files (RHEL)
**By: Sumit Nikumbh**  
📅 Date: 13/07/2025

---

## 1️⃣ Why We Need File Access Control

In Linux, controlling access to files and directories is essential for:

- System security  
- Multi-user environment integrity  
- Data privacy  

Using **permissions**, we control who can read, write, or execute a file or directory — ensuring only authorized users can perform specific actions.

---

## 2️⃣ Difference of `rwx` for Files and Directories

| Permission | File Behavior           | Directory Behavior              |
|------------|-------------------------|---------------------------------|
| `r` (read) | View file contents      | List contents (`ls`)           |
| `w` (write)| Modify the file         | Add/remove/rename files        |
| `x` (exec) | Run file as a program   | Enter directory (`cd`)         |

---

## 3️⃣ Default Permissions of Files and Directories

- **Files:** `666` → Read and write for all  
- **Directories:** `777` → Read, write, and execute for all  

> These defaults are **masked by the umask** to determine the final permission.

---

## 4️⃣ `/etc/passwd`: 7 Fields of a User

Each line in `/etc/passwd` describes a user and contains 7 fields separated by colons `:`:

```bash

username:password:UID:GID:GECOS:home_directory:shell
Field	Description
1.username	Login name of the user
2.password	Placeholder (x) if password is stored in /etc/shadow
3.UID	User ID — unique number assigned to each user
4.GID	Primary Group ID — links to /etc/group
5.GECOS	User's full name or description (optional field for comments)
6.home_directory The path to the user's home directory (e.g., /home/sumit)
7.shell	The default shell assigned (e.g., /bin/bash)

5️⃣ /etc/group: 4 Fields of a Group
Each line in /etc/group defines a group and contains 4 fields:

group_name:password:GID:user_list
Field	Description
1.group_name	Name of the group (e.g., developers)
2.password	Group password (rarely used, often x)
3.GID	Group ID — unique numeric identifier
4.user_list	Comma-separated list of users who are members of the group

6️⃣ File Type Symbols in ls -l
Symbol	Type
-	Regular file
d	Directory
l	Symbolic link
c	Character device
b	Block device
s	Socket
p	Named pipe

7️⃣ rwx Meaning & Numeric Values
Symbol	Value
r	4
w	2
x	1
-	0
Example: rwxr-xr-- = 755

8️⃣ chmod with u/g/o/a and Operators
u → user (owner)

g → group

o → others

a → all (user+group+others)

Operators:

= → set exact permissions

+ → add permission

- → remove permission

Examples:
bash
chmod u+x filename
chmod g-w filename
chmod a=rwx filename

9️⃣ chmod with Numeric Values
bash
chmod 755 filename

7 = rwx (user)

5 = r-x (group)

5 = r-x (others)

🔟 umask – Default Permission Mask
🔎 What is umask?
umask (user file creation mode mask) defines default permission settings for new files and directories.
It subtracts permissions from the system defaults:

Files default to: 666 (rw-rw-rw-)

Directories default to: 777 (rwxrwxrwx)

🧮 How it Works
bash
Final Permission = Default Permission - umask
🧑‍💻 Default umask Values
User Type	Default umask	Resulting File Perm	Resulting Dir Perm
Regular user	002	664 (rw-rw-r--)	775 (rwxrwxr-x)
Root user	022	644 (rw-r--r--)	755 (rwxr-xr-x)

🔧 View Current umask
bash
umask
🕐 Temporary Change
bash
umask 027
Applies only to the current shell session.

🔒 Permanent Change
For Individual User:
Edit:

bash
~/.bashrc

or

bash
~/.bash_profile
Add:
bash
umask 027

For All Users (System-wide):
Edit the file:
bash
/etc/profile

Or:

bash
/etc/bashrc
Add:
bash
umask 027

1️⃣1️⃣ chown – Change Ownership
bash
chown newuser filename
chown user:group filename
chown -R user directory/

1️⃣2️⃣ chgrp – Change Group Ownership
bash
chgrp newgroup filename
chgrp -R group directory/

1️⃣3️⃣ Special Permissions: SUID, SGID, Sticky Bit
🧍 SUID (Set User ID)
Executes file with file owner's permissions.

Set: chmod u+s filename

Shown as: s in user execute (e.g., -rwsr-xr-x)

👥 SGID (Set Group ID)
Files created in directory inherit group.

Set: chmod g+s directory

Shown as: s in group execute (e.g., drwxr-sr-x)

📌 Sticky Bit
Only file owner can delete their files in directory.

Common in /tmp.

Set: chmod +t directory

Shown as: t in other execute (e.g., drwxrwxrwt)

1️⃣4️⃣ How to Check Special Permissions
bash
ls -l
stat filename

1️⃣5️⃣ Recursive Operations & Special Cases
bash
chmod -R o+X directory/
chmod -R o+x directory/
chown -R user directory/
chgrp -R group directory/

1️⃣6️⃣ chage – Password Expiry Management
The chage command is used to view and configure user password aging policies.

🔎 View password aging settings:
bash
chage -l username
⚙️ Modify password aging settings:
Option	Description
-d	Set last password change date (YYYY-MM-DD)
-E	Set account expiry date (YYYY-MM-DD or -1 for no expiry)
-I	Set number of inactive days after password expires
-m	Set minimum number of days between password changes
-M	Set maximum number of days before password must be changed
-W	Set number of warning days before password expiration

🧪 Example:
bash
chage -M 90 -m 7 -W 10 sumit
User sumit must change the password every 90 days, not sooner than 7 days, and will get a warning 10 days before expiration.

