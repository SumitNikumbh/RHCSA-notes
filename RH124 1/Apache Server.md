# **Proof of Concept: Hosting Apache Server from User’s Home Directory**

**By: Sumit Nikumbh**

![](file:///C:/Users/SUMIT/AppData/Local/Temp/msohtmlclip1/01/clip_image001.png)

## **Objective**

**This PoC demonstrates how to host a webpage using Apache HTTP Server from a **normal user’s home directory** in RHEL. The example uses user **sumit**.**

![](file:///C:/Users/SUMIT/AppData/Local/Temp/msohtmlclip1/01/clip_image002.png)

## **Steps**

### **1. Verify Available Disks**

```
lsblk
```

- **Lists block devices (hard disks, partitions, DVD).**
- **Ensures** `**/dev/sr0**` **(RHEL DVD) is available.**

![](file:///C:/Users/SUMIT/AppData/Local/Temp/msohtmlclip1/01/clip_image002.png)

### **2. Mount RHEL DVD for Local Yum Repository**

```
mount /dev/sr0 /mnt
```

- **Mounts the DVD on** `**/mnt**`**.**
- **Required for configuring a local repository.**

![](file:///C:/Users/SUMIT/AppData/Local/Temp/msohtmlclip1/01/clip_image002.png)

### **3. Configure Local Yum Repository**

```
vim /etc/yum.repos.d/sumit.repo
```

**Example content inside** `**sumit.repo**
![pitucre 1 ](./Attachments/apachepicture1.png)

- **Creates a custom repo file pointing to RHEL DVD.**

![](file:///C:/Users/SUMIT/AppData/Local/Temp/msohtmlclip1/01/clip_image005.png)

### **4. Update Yum Cache**

```
yum update
```

- **Refreshes metadata for the newly added repo.**

![](file:///C:/Users/SUMIT/AppData/Local/Temp/msohtmlclip1/01/clip_image005.png)

### **5. Install Apache HTTP Server**

```
yum install httpd* -y
```
![pitucre 2 ](./Attachments/apachepicture2.png)

- **Installs Apache web server and required packages.**

![](file:///C:/Users/SUMIT/AppData/Local/Temp/msohtmlclip1/01/clip_image002.png)

### **6. Enable and Start Apache Service**

```
systemctl enable --now httpd
```

```
systemctl status httpd
```
![picture 3 ](apachepicture3.png)
- **Enables Apache at boot and starts it immediately.**

![](file:///C:/Users/SUMIT/AppData/Local/Temp/msohtmlclip1/01/clip_image001.png)

### **7. Create User Directory for Hosting**

```
mkdir /home/sumit/public_html
```

```
echo "this is apache server" > /home/sumit/public_html/sumit.html
```

- **Creates** `**public_html**` **folder inside user home.**
- **Creates a test HTML file.**

![](file:///C:/Users/SUMIT/AppData/Local/Temp/msohtmlclip1/01/clip_image002.png)

### **8. Set Proper File Permissions**

```
chmod 755
```

```
chmod 755
```

```
chmod 644
```

- **Ensures Apache can read files inside user directory.**

![](file:///C:/Users/SUMIT/AppData/Local/Temp/msohtmlclip1/01/clip_image002.png)

### **9. Configure User Directory Access in Apache**

**Edit** `**userdir.conf**`**:**

```
vim /etc/httpd/conf/userdir.conf
```
![picture 4 ](apachepicture4.png)
- **1****. Add # infront of UserDir Disable (to make it comment).**
- 2. Remove # infront of UserDir public_html (enables public_html).**
- **3****. If we want to host different file add it here  <directory “/home/*/filename”>**

![](file:///C:/Users/SUMIT/AppData/Local/Temp/msohtmlclip1/01/clip_image012.png)

### **10. Configure SELinux for Apache**

**Enable home directory access:**

```
setsebool -P httpd_enable_homedirs on
```

**Set SELinux context:**

```
semanage fcontext -a -t httpd_sys_content_t '/home/sumit/public_html(/.*)?'
```

```
restorecon -R /home/sumit/public_html
```

**Verify labels:**

```
ls -lZd /home/sumit/public_html
```

```
ls -lZ /home/sumit/public_html/sumit.html
```

```
It should be httpd_user_content_t 
```

![](file:///C:/Users/SUMIT/AppData/Local/Temp/msohtmlclip1/01/clip_image002.png)

### **11. Restart Apache Service**

```
systemctl restart httpd
```

- **Reloads Apache with new configurations.**

![](file:///C:/Users/SUMIT/AppData/Local/Temp/msohtmlclip1/01/clip_image002.png)

### **12. Verify IP Address**

```
ip a
```

- **Get the server IP address (example:** `**192.168.179.133**`**).**

![](file:///C:/Users/SUMIT/AppData/Local/Temp/msohtmlclip1/01/clip_image002.png)

### **13. Test Website Access**

```
curl http://192.168.179.133/~sumit/sumit.html
```

 **Accesses the hosted page from user’s directory.**

![](file:///C:/Users/SUMIT/AppData/Local/Temp/msohtmlclip1/01/clip_image002.png)

## **Result**

**Apache is successfully hosting content from the **user’s home directory** (**`**/home/sumit/public_html/sumit.html**`**).**

![](file:///C:/Users/SUMIT/AppData/Local/Temp/msohtmlclip1/01/clip_image012.png)