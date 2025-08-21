# Proof of Concept (PoC) Document

Topic: Managing Users and Groups

Chapter: RH124 v9 – Chapter 6

Prepared by: Sumit Nikumbh

Date: 08/07/2025

## Executive Summary

This Proof of Concept demonstrates how to manage users and groups on a Red Hat Enterprise Linux 9 system. It includes commands for creating, modifying, and deleting users and groups. It also covers password management, default settings, and user environment configuration.

## Objective

- Learn to create, modify, and delete users and groups.

- Understand user-related configuration files and home directory structure.

- Apply password policies and default group behaviors.

## Scope

- User and group management commands.

- Password and account configuration.

- Understanding UID/GID and related configuration files.

## Requirements

- Operating System: RHEL 9

- User Privileges: Root access

- Tools Used: useradd, usermod, userdel, groupadd, passwd, id, su, etc.

## Methodology / Approach

**Types of users :**

**ROOT USER**: the superuser with UID 0, having full administrative control over the system.

**REGULAR USER**: a human-created user with UID 1000 or above, used for standard loging and personal tasks.

**SYSTEM USER**: A non-login service account with UID 1-999 used by system processes and background services.

**Imp directory paths**  **:**

/etc/passwd : user info

/etc/group : group info

/etc/shadow : user password info

/etc/gshadow : groups password info

**ALL IMP COMMANDS USERS :-**

1.     **Useradd :** To create a new user ; **passwd** to give password to a user.

 In this snapshot we see user U1 is created and password is set for that user using **passwd.**

2.     **Su - <username>** : to switch from one user to another.

In this snapshot we can see switch from root user to normal user is done without asking of password [ **su – U1** ] but switching from normal user to root (superuser) it ask password after giving password we can switch to root user [**su – root**]

**3.**     **Id <username>** : The id command is used to display user ID (UID) , group ID (GID) , and group membership information of current or specific user.

       **Id** command showing current user info ; **id U1** showing that specific user info.

4.     **Who** : this command is used to display information about users currently logged into the system.

snapshot showing users logged info.

5.     **Whoami** : to see current user .

 Snapshot showing current user .

6.     **Userdel** is used to delete a user ; **userdel -r** is used to delete user with its home directory.

We see user U1 is deleted but is home directory remains

We used **rm -rf /home/U1** to delete its home directory we use this command when user is removed but we want to delete its directory.

In this we used **userdel -r** using this helps us deleting the user with its home directory.

7.     Options for modifying the user :

1.-u : used for changing user ID (UID) , **usermod -u <UID that we want to assign> <username>.**

2.-l : used for changing users name , **usermod -l <oldname> <newname> .**

3.-g : used for changing group ID (GID) ,  **usermod -g <GID that want to assign> <username> .**

4.-d : used for changing home directory of user, **usermod -m -d <newpath>       <username>**

5.-s : used for changing user shell (bash) , **usermod -s /sbin/nologin <username> .**

6.-L : used for locking user , **usermod -L <username> .**

7.-U : used for unlocking the locked user , **usermod -U <username> .**

8.-c : used for changing comment of user,  **usermod -c <username> .**

9.-G : used for adding a user to group , **usermod -aG <groupname> <username>,** (-a is used for append) .

**TYPES OF GROUPS :**

**PRIMARY GROUP** : the primary group is automatically assigned to files and directories created by the user, a user can only have one primary group.

**SECONDARY GROUP** : secondary groups grant additional permissions to access shared files or services, users can be part of multiple secondary groups.

 **ALL IMP COMMANDS GROUPS :**

1.     **Groupadd** : used for creating a group & **gpasswd** : used for giving group password.

2.     **gpasswd -d <username> <groupname>** : used for removing a specific user form a group.

We can see first chrollo was in group troupe after using the command chrollo was removed from group troupe .

3.     **groups <username>** : this command lists all groups the user belong to.

In this we can see chrollo is in chrollo group which is its self primary group and troupe and phantom groups which are the secondary groups.

4.     **groupadd -g <GID> <groupname>** : this command is to create a group with a specific GID .

      The group newgroup is created with a GID of 1111 .

**PASSWORDS** : passwd command all options :

**Passwd -e <username>** :  forces user to change password on the next login .

**Passwd -M 99999 <username>** : prevents password expiry .

**Passwd -x <days> <username>** : set a max number of days a password is valid .

**Passwd -n <days> <username>** : set minimum days before user cane change password .

**Passwd -w <days> <username>** : set warning days before password expiration .

**GETENT** : The getent command is used to fetch entries from system databases configured in /etc/nsswitch.conf, such as users, groups, hosts, services, etc.

1. **getent passwd** : list all users from /etc/passwd .

2. **getent passwd** <username> : shows that specific user’s detail .

3. **getent group** : lists all groups from /etc/group.

4.**Getent group <groupname>** : shows details of that group.

**Getent** : is an powerful tool to view such details cat /etc/passwd only shows local users but getent shows all local users and remote users.

## Results / Findings

- Successfully created and managed users and groups.

- Applied user policies and verified configurations.

## Challenges Faced

- Some commands required root privileges.

- Conflicts while re-creating users with the same name without deleting the home directory.

## Conclusion

This PoC successfully demonstrates the management of users and groups on RHEL 9. It provides foundational knowledge for Linux system administration tasks related to access control and user environment setup.

## Appendix - Useful Commands Summary

- useradd, usermod, userdel, groupadd, groupdel

- passwd, id, su, whoami, groups, getent

- cat /etc/passwd, cat /etc/group, cat /etc/shadow