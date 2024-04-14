##### Jenkins Install on EC2 Instance

***Prerequisites***
To follow this tutorial, you will need:
1. Ubuntu Server 22.04 server configured with a non-root user and firewall.
2. OpenJDK 11 or upper

###### Java & OpenJDK Install
###### [Open JDK Install](https://openjdk.org)

```bash
apt install openjdk-21-jre-headless
find /usr/lib/jvm/java-21-openjdk-arm64/java* | head -n 3 # searching if required
nano .bashrc # or .bash_profile or .zshrc (mac)
JAVA_HOME=/usr/lib/jvm/java-21-openjdk-arm64 # set java path variable
PATH=$PATH:$HOME/bin:$JAVA_HOME/bin
export PATH
source .bashrc # or .bash_profile or .zshrc (mac)
java --version
echo $PATH
echo $JAVA_HOME
```

Or (my recommendation )

```bash
apt update
java -version
cd /opt
# as per your OS you should download specified version. my case aarch64.
wget https://download.java.net/java/GA/jdk21.0.2/f2283984656d49d69e91c558476027ac/13/GPL/openjdk-21.0.2_linux-aarch64_bin.tar.gz
tar zxvf openjdk-21.0.2_linux-aarch64_bin.tar.gz
mv jdk-21.0.2 jdk-21
whereis java # searching if required
nano .bashrc # or .bash_profile or .zshrc (mac)
JAVA_HOME=/opt/jdk-21 # set java path variable
PATH=$PATH:$HOME/bin:$JAVA_HOME/bin
export PATH
source .bashrc # or .bash_profile or .zshrc (mac)
java --version
echo $PATH
echo $JAVA_HOME
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