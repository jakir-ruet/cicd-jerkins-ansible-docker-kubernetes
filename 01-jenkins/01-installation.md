##### Jenkins Install on EC2 Instance

***Prerequisites***
To follow this tutorial, you will need:
1. Ubuntu Server 22.04 server configured with a non-root user and firewall.
2. OpenJDK 11 or upper

###### Java & OpenJDK Install
###### [Open JDK Install](https://openjdk.org)

```bash
apt update
java -version
apt install openjdk-21-jre-headless # for 21 version (latest) - Recommended
apt install default-jre # for 11 version
java -version
```

###### [JDK Install (not for Jenkins)](https://www.oracle.com/java/technologies/downloads/)
It is for  machine
```bash
wget https://download.oracle.com/java/21/latest/jdk-21_linux-x64_bin.deb
dpkg -i jdk-21_linux-x64_bin.deb
java -version
```

***For ARM machine*** 

```bash
echo "$PATH"  # checking the previous path
wget https://download.oracle.com/java/21/latest/jdk-21_linux-aarch64_bin.tar.gz
tar xvf jdk-21_linux-aarch64_bin.tar.gz
mv jdk-21.0.2 /usr/local/
ls -a
nano .profile
export PATH="$PATH:$HOME/usr/local/jdk-21.0.2/bin"
```

It make variables available in your current shell session.

```bash
source ~/.profile
echo $PATH
java --version
```

###### [Install](https://www.jenkins.io/doc/book/installing/linux/) Jenkins LTS.
```bash
wget -O /usr/share/keyrings/jenkins-keyring.asc \
  https://pkg.jenkins.io/debian-stable/jenkins.io-2023.key
echo deb [signed-by=/usr/share/keyrings/jenkins-keyring.asc] \
  https://pkg.jenkins.io/debian-stable binary/ | tee \
  /etc/apt/sources.list.d/jenkins.list > /dev/null
```
```bash
apt-get update
apt-get install jenkins
```
```bash

```
Firewall Configuration
```bash
ufw allow 8080
ufw allow OpenSSH
ufw enable
ufw status
```

Use the following command to get the password
```bash
cat /var/lib/jenkins/secrets/initialAdminPassword
```
After getting the initial password and put required information & selecting plugins we will configure jenkins.
```bash
http://localhost:8080
```
Add JDK variable under the tools menu on Jenkins
```bash
/usr/lib/jvm/java-1.21.0-openjdk-amd64
JAVA_HOME
```
