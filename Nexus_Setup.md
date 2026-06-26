# 🚀 Nexus Repository Manager Installation on AWS EC2

## 📌 Project Overview

This project demonstrates the installation and configuration of **Nexus Repository Manager 3.92.3-01 Community Edition** on an AWS EC2 Red Hat Linux server.

Nexus is used to store and manage build artifacts such as **JAR** and **WAR** files generated during CI/CD pipelines.

## 🛠️ Tools Used

* AWS EC2 (Red Hat Linux)
* Java 21
* Nexus Repository Manager 3
* Linux

---

# 🔧 Nexus Installation Steps

## Step 1: Switch to Root User

```bash
sudo su -
```

---

## Step 2: Move to `/opt` Directory

```bash
cd /opt
```

Verify downloaded package:

```bash
ls -lrth
```

```bash
yum install tar wget tree -y
```

```bash
wget https://download.sonatype.com/nexus/3/nexus-3.92.3-01-linux-x86_64.tar.gz
```

Expected Output:

```text
nexus-3.92.3-01-linux-x86_64.tar.gz
```

---

## Step 3: Extract Nexus Package

```bash
tar -xzvf nexus-3.92.3-01-linux-x86_64.tar.gz
```

Rename extracted directory:

```bash
mv /opt/nexus-3.92.3-01 /opt/nexus
```

Verify:

```bash
ls -lrth
```

Expected Output:

```text
nexus
sonatype-work
```

---

## Step 4: Create Nexus User

For security reasons, Nexus should not run as the root user.

```bash
useradd nexus
```

Grant sudo access:

```bash
visudo
```

Add the following entry:

```text
nexus ALL=(ALL) NOPASSWD: ALL
```

---

## Step 5: Assign Permissions

```bash
chown -R nexus:nexus /opt/nexus
chown -R nexus:nexus /opt/sonatype-work

chmod -R 775 /opt/nexus
chmod -R 775 /opt/sonatype-work
```

---

## Step 6: Configure Nexus Runtime User

Edit the Nexus configuration file:

```bash
vi /opt/nexus/bin/nexus.rc
```

Add:

```text
run_as_user="nexus"
```

Save and exit.

---

# ⚠️ Issue Faced During Setup

### Attempted Command

```bash
ln -s /opt/nexus/bin/nexus /etc/init.d/nexus
```

### Error

```text
ln: failed to create symbolic link '/etc/init.d/nexus':
No such file or directory
```

### Reason

Modern Red Hat and Amazon Linux distributions use **systemd** instead of the older **init.d** service management system.

Therefore, this step is not required for manually starting Nexus.

---

# 🚀 Starting Nexus

Switch to the Nexus user:

```bash
su - nexus
```

Move to the Nexus bin directory:

```bash
cd /opt/nexus/bin
```

Verify files:

```bash
ls -l
```

Expected Output:

```text
nexus
nexus.rc
nexus.vmoptions
sonatype-nexus-repository-3.92.3-01.jar
```

Start Nexus:

```bash
./nexus start
```

Output:

```text
Starting nexus
```

---

# 🔍 Verify Nexus Startup

Navigate to log directory:

```bash
cd /opt/sonatype-work/nexus3/log
```

View logs:

```bash
tail -50 nexus.log
```

Look for the following success messages:

```text
Started ServerConnector ... {0.0.0.0:8081}

Started Sonatype Nexus COMMUNITY 3.92.3-01
```

These messages confirm that Nexus has started successfully.

---

# 🔌 Verify Nexus Port

Check whether Nexus is listening on port **8081**:

```bash
ss -tulnp | grep 8081
```

Expected Output:

```text
tcp LISTEN *:8081
```

This confirms Nexus is running successfully.

---

# 🌐 Access Nexus UI

Enable **Port 8081** in the EC2 Security Group.

Open browser:

```text
http://<EC2-PUBLIC-IP>:8081
```

Example:

```text
http://15.207.110.48:8081
```

---

# 🔑 Get Initial Admin Password

```bash
cat /opt/sonatype-work/nexus3/admin.password
```

Example Output:

```text
c8c18846-b4ee-429d-9975-72e0343da053
```

Login Credentials:

```text
Username : admin
Password : <generated password>
```

---

# 🔒 Change Admin Password

After first login:

```text
New Password     : admin@123
Confirm Password : admin@123
```

---

# 📚 Skills Practiced

* AWS EC2
* Linux Administration
* Java 21
* Nexus Repository Manager
* Artifact Repository Management
* User & Permission Management
* Service Troubleshooting
* Log Analysis

---

# 🎯 Outcome

✅ Nexus Repository Manager Installed Successfully

✅ Nexus Running on Port 8081

✅ Admin Access Configured

✅ Artifact Repository Ready for CI/CD Integration

---

# 🔄 CI/CD Pipeline Flow

```text
GitHub
   ↓
Jenkins
   ↓
Maven
   ↓
SonarQube
   ↓
Nexus
   ↓
Docker
   ↓
Tomcat
```

## 🚀 Next Steps

* Integrate Maven with Nexus
* Upload WAR/JAR artifacts to Nexus
* Configure Jenkins to publish artifacts automatically
* Integrate Docker image build and deployment
* Deploy application on Tomcat
  
Sonatype Nexus Interface
<img width="1339" height="674" alt="Screenshot 2026-06-24 224450" src="https://github.com/user-attachments/assets/adee3334-3a35-40cf-b329-fab773c2ceb2" />

AWS EC2 Nexus Instance 
<img width="1357" height="235" alt="Screenshot 2026-06-24 224546" src="https://github.com/user-attachments/assets/d378d5dc-86f8-4ee7-92a0-db9783df8fbf" />

AWS EC2 Nexus Instance -> security group -> inbound rule -> 8081 port enable
<img width="1350" height="460" alt="Screenshot 2026-06-24 224515" src="https://github.com/user-attachments/assets/bb7173fc-08d0-4110-8263-46f5cb288baf" />

