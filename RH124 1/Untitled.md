# **Proof of Concept: Hosting Apache Server from User’s Home Directory**
**By: Sumit Nikumbh**

---

## **Objective**

This PoC demonstrates how to host a webpage using the Apache HTTP Server from a **normal user’s home directory** in **RHEL**.  
The example uses the user: **`sumit`**

---

## **Steps**

### **1. Verify Available Disks**

```bash
lsblk
Lists block devices (hard disks, partitions, DVD).

Ensure /dev/sr0 (RHEL DVD) is available.

2. Mount RHEL DVD for Local Yum Repository
bash
Copy code
mount /dev/sr0 /mnt
Mounts the DVD on /mnt, required to configure a local repository.

3. Configure Local Yum Repository
bash
Copy code
vim /etc/yum.repos.d/sumit.repo
Example content for sumit.repo:

ini
Copy code
[BaseOS]
name=Red Hat Enterprise Linux 9 - BaseOS
baseurl=file:///mnt/BaseOS
enabled=1
gpgcheck=0

[AppStream]
name=Red Hat Enterprise Linux 9 - AppStream
baseurl=file:///mnt/AppStream
enabled=1
gpgcheck=0
📸 apachepicture1.png
![[apachepicture1.png]]

4. Update Yum Cache
bash
Copy code
yum update
Refreshes metadata for the newly added local repository.

5. Install Apache HTTP Server
bash
Copy code
yum install httpd* -y
📸 apachepicture2.png
![[apachepicture2.png]]

6. Enable and Start Apache Service
bash
Copy code
systemctl enable --now httpd
systemctl status httpd
📸 apachepicture3.png
![[apachepicture3.png]]

Enables Apache at boot and starts it immediately.

7. Create User Directory for Hosting
bash
Copy code
mkdir /home/sumit/public_html
echo "This is Apache server" > /home/sumit/public_html/sumit.html
Creates public_html folder in user’s home directory.

Creates a basic HTML file for testing.

8. Set Proper File Permissions
bash
Copy code
chmod 755 /home/sumit
chmod 755 /home/sumit/public_html
chmod 644 /home/sumit/public_html/sumit.html
Ensures Apache has read access to the directory and files.

9. Configure Apache to Allow User Directories
Edit the file:

bash
Copy code
vim /etc/httpd/conf.d/userdir.conf
Make the following changes:

Comment out the line:

apache
Copy code
#UserDir disabled
Uncomment:

apache
Copy code
UserDir public_html
Optionally, customize the directory block:

apache
Copy code
<Directory "/home/*/public_html">
    AllowOverride FileInfo AuthConfig Limit
    Options MultiViews Indexes SymLinksIfOwnerMatch IncludesNoExec
    Require method GET POST OPTIONS
</Directory>
📸 apachepicture4.png
![[apachepicture4.png]]

10. Configure SELinux for User Directory Hosting
Enable home directory access:

bash
Copy code
setsebool -P httpd_enable_homedirs on
Set SELinux context for public_html:

bash
Copy code
semanage fcontext -a -t httpd_sys_content_t '/home/sumit/public_html(/.*)?'
restorecon -R /home/sumit/public_html
Verify SELinux context:

bash
Copy code
ls -lZd /home/sumit/public_html
ls -lZ /home/sumit/public_html/sumit.html
Should show httpd_sys_content_t or httpd_user_content_t.

If semanage is not available, install policycoreutils:

bash
Copy code
yum install policycoreutils-python-utils -y
11. Restart Apache Service
bash
Copy code
systemctl restart httpd
Applies all configuration changes.

12. Verify IP Address
bash
Copy code
ip a
Note the IP address to test web access (e.g., 192.168.179.133).

13. Test Webpage Access
bash
Copy code
curl http://192.168.179.133/~sumit/sumit.html
Should return: This is Apache server

You can also access it via a browser:
http://<your-ip>/~sumit/sumit.html

Result
Apache is successfully hosting content from the user's home directory:
/home/sumit/public_html/sumit.html


# 📄 Proof of Concept: Hosting Apache Server from User’s Home Directory
**By: Sumit Nikumbh**

---

## 🧭 Objective

This Proof of Concept (PoC) demonstrates how to host a webpage using the **Apache HTTP Server** from a **normal user's home directory** on **RHEL**.  
Example user: `sumit`

---

## 🛠️ Steps

### 1. 🔍 Verify Available Disks

```bash
lsblk
Lists block devices.

Ensure /dev/sr0 (RHEL installation DVD) is available.

2. 💿 Mount RHEL DVD for Local Yum Repository
bash
Copy code
mount /dev/sr0 /mnt
Mounts the RHEL media to /mnt.

3. 📁 Configure Local Yum Repository
Create a new repo file:

bash
Copy code
vim /etc/yum.repos.d/sumit.repo
Example content:

ini
Copy code
[BaseOS]
name=RHEL9 BaseOS
baseurl=file:///mnt/BaseOS
enabled=1
gpgcheck=0

[AppStream]
name=RHEL9 AppStream
baseurl=file:///mnt/AppStream
enabled=1
gpgcheck=0
📸 ![[apachepicture1.png]]

4. 🔄 Update Yum Cache
bash
Copy code
yum update
Updates metadata using the new repository.

5. 🌐 Install Apache HTTP Server
bash
Copy code
yum install httpd* -y
📸 ![[apachepicture2.png]]

6. 🚀 Enable and Start Apache
bash
Copy code
systemctl enable --now httpd
systemctl status httpd
📸 ![[apachepicture3.png]]

7. 🏠 Create User Directory for Hosting
bash
Copy code
mkdir /home/sumit/public_html
echo "This is Apache server" > /home/sumit/public_html/sumit.html
8. 🔐 Set Proper File Permissions
bash
Copy code
chmod 755 /home/sumit
chmod 755 /home/sumit/public_html
chmod 644 /home/sumit/public_html/sumit.html
Grants Apache read access to files.

9. ⚙️ Configure Apache for User Directories
Edit the config file:

bash
Copy code
vim /etc/httpd/conf.d/userdir.conf
Make the following changes:

Comment this line:

apache
Copy code
#UserDir disabled
Uncomment this:

apache
Copy code
UserDir public_html
Optional for advanced config:

apache
Copy code
<Directory "/home/*/public_html">
    AllowOverride FileInfo AuthConfig Limit
    Options MultiViews Indexes SymLinksIfOwnerMatch IncludesNoExec
    Require method GET POST OPTIONS
</Directory>
📸 ![[apachepicture4.png]]

10. 🔒 Configure SELinux
Enable home directory access:

bash
Copy code
setsebool -P httpd_enable_homedirs on
Set SELinux context:

bash
Copy code
semanage fcontext -a -t httpd_sys_content_t '/home/sumit/public_html(/.*)?'
restorecon -R /home/sumit/public_html
Check the labels:

bash
Copy code
ls -lZd /home/sumit/public_html
ls -lZ /home/sumit/public_html/sumit.html
Expected context: httpd_sys_content_t or httpd_user_content_t

If semanage is not installed:

bash
Copy code
yum install policycoreutils-python-utils -y
11. 🔁 Restart Apache
bash
Copy code
systemctl restart httpd
12. 🌐 Verify IP Address
bash
Copy code
ip a
Example: 192.168.179.133

13. ✅ Test Webpage Access
From terminal:

bash
Copy code
curl http://192.168.179.133/~sumit/sumit.html
Or open in browser:

arduino
Copy code
http://192.168.179.133/~sumit/sumit.html
🧪 Result
Apache is successfully hosting content from:
/home/sumit/public_html/sumit.html

