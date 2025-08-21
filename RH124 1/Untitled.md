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
mount /dev/sr0 /mnt
Mounts the DVD on /mnt, required to configure a local repository.

3. Configure Local Yum Repository
bash
vim /etc/yum.repos.d/sumit.repo
Example content for sumit.repo:

ini
[BaseOS]
name=BaseOS
baseurl=file:///mnt/BaseOS
enabled=1
gpgcheck=0

[AppStream]
name=AppStream
baseurl=file:///mnt/AppStream
enabled=1
gpgcheck=0

4. Update Yum Cache
bash
yum update
Refreshes metadata for the newly added local repository.

5. Install Apache HTTP Server
bash
yum install httpd* -y

6. Enable and Start Apache Service
bash
systemctl enable --now httpd
systemctl status httpd
Enables Apache at boot and starts it immediately.

7. Create User Directory for Hosting
bash
mkdir /home/sumit/public_html
echo "This is Apache server" > /home/sumit/public_html/sumit.html
Creates public_html folder in user’s home directory.

Creates a basic HTML file for testing.

8. Set Proper File Permissions
bash
chmod 755 /home/sumit
chmod 755 /home/sumit/public_html
chmod 644 /home/sumit/public_html/sumit.html
Ensures Apache has read access to the directory and files.

9. Configure Apache to Allow User Directories
Edit the file:

bash
vim /etc/httpd/conf.d/userdir.conf
Make the following changes:

1.Comment out the line:
#UserDir disabled

2.Uncomment:
UserDir public_html

3.Optionally, customize the directory block:

<Directory "/home/*/public_html">
    AllowOverride FileInfo AuthConfig Limit
    Options MultiViews Indexes SymLinksIfOwnerMatch IncludesNoExec
    Require method GET POST OPTIONS
</Directory>

10. Configure SELinux for User Directory Hosting
Enable home directory access:

bash
setsebool -P httpd_enable_homedirs on
Set SELinux context for public_html:

bash
semanage fcontext -a -t httpd_sys_content_t '/home/sumit/public_html(/.*)?'
restorecon -R /home/sumit/public_html

Verify SELinux context:
bash
ls -lZd /home/sumit/public_html
ls -lZ /home/sumit/public_html/sumit.html

Should show httpd_sys_content_t or httpd_user_content_t.

If semanage is not available, install policycoreutils:
bash
yum install policycoreutils-python-utils -y

11. Restart Apache Service
bash
systemctl restart httpd
Applies all configuration changes.

12. Verify IP Address
bash
ip a
Note the IP address to test web access (e.g., 192.168.179.133).

13. Test Webpage Access
bash
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

