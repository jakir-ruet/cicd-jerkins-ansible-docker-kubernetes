[![LinkedIn][linkedin-shield-lapissoft]][linkedin-url-lapissoft]
[![Facebook-Page][facebook-shield-lapissoft]][facebook-url-lapissoft]
[![Youtube][youtube-shield-lapissoft]][youtube-url-lapissoft]

## Visit Us [Lapis Soft](http://www.lapissoft.com)

#### The complete DevOps project using Jenkins, Ansible, Docker as well as Kubernetes on AWS.

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

##### Ansible Install on EC2 Instance (A Control & Managed)

``` bash
add-apt-repository ppa:ansible/ansible
```
``` bash
sudo apt-get install ansible
```

``` bash
ansible --version
```
Active the bash completion support we may install these.
``` bash
apt install python3-argcomplete
activate-global-python-argcomplete3
```
To allow nodes into the ansible server, create a group on the 'hosts' file. A group show as follows;
``` bash
nano /etc/ansible/hosts
```
``` bash
[AnsibleGroup] # Group Name
172.16.102.130 # Node #1 IP
```
And update the ansible.cfg file
``` bash
nano /ete/ansible/ansible.cfg
```
Add/Update the following line in ansible.cfg file.
``` bash
[defaults]
inventory = /etc/ansible/hosts
sudo_user = root
```
Create a user in three instances
``` bash
adduser ansible-usr
passwd 054003
```
To give 'sudo privileges' to 'ansible-usr' user in node instances.
``` bash
sudo visudo
```
And put this line below %admin user of three instances
```bash
%ansible-usr ALL=(ALL) NOPASSWD:ALL
```
Login as ansible-usr user into three instances
```bash
su - ansible-usr
```
```bash
sudo apt-get update
```
Try to connect node instances
```bash
ssh 172.16.102.130 # Node #1 IP
```
Here, access is denied, or a password is required. For this purpose, we uncomment/change the sshd_config file on three instances. It must be done under the root user of these instances.
```bash
sudo nano /etc/ssh/sshd_config
```
```bash
PubkeyAuthentication yes
PasswordAuthentication yes
```
Restart the ssh service & check status
```bash
service ssh restart
```
```bash
service ssh status
```
Login as 'ansible-usr' into ansible server.
```bash
su - ansible-usr # in server
```
Try to access (two) node instances from the ansible server under the 'ansible-usr'. This is very disturbing because it is asking for the pass for each login.
```bash
ssh 172.16.102.130 # node IP
```

Disable the password asking each time while logging the ansible server (ansible-usr) to the node instance by generating ssh-key.
```bash
ssh-keygen # not use the password
ls -a
cd .ssh
ssh-copy-id ansible-usr@172.16.102.132 # from server instance (here 172.16.102.130 is node 1 IP)
```

#### Ad-Hoc
Checking the all connected node
```bash
ansible all -m ping
```
Checking specific node group
```bash
ansible AnsibleGroup -m ping
```
Checking specific node group and specific node using index
```bash
ansible AnsibleGroup[0] -m ping
```

##### File operations
Checking available hosts
```bash
ansible all --list-hosts
```
Checking all connected node's information
```bash
ansible all -m gather_facts
```
Checking specific connected node's information
```bash
ansible all -m gather_facts --limit NodeIPAddress
```

##### Docker Installation on EC2 Instance

Add Docker's official GPG key:

```bash
sudo apt-get update
sudo apt-get install ca-certificates curl
sudo install -m 0755 -d /etc/apt/keyrings
sudo curl -fsSL https://download.docker.com/linux/ubuntu/gpg -o /etc/apt/keyrings/docker.asc
sudo chmod a+r /etc/apt/keyrings/docker.asc
```

Add the repository to Apt sources:
```bash
echo \
  "deb [arch=$(dpkg --print-architecture) signed-by=/etc/apt/keyrings/docker.asc] https://download.docker.com/linux/ubuntu \
  $(. /etc/os-release && echo "$VERSION_CODENAME") stable" | \
  sudo tee /etc/apt/sources.list.d/docker.list > /dev/null
sudo apt-get update
```

To install the latest version, run:
```bash
sudo apt-get install docker-ce docker-ce-cli containerd.io docker-buildx-plugin docker-compose-plugin
```

## Courtesy of Jakir

[![LinkedIn][linkedin-shield-jakir]][linkedin-url-jakir]
[![Facebook-Page][facebook-shield-jakir]][facebook-url-jakir]
[![Youtube][youtube-shield-jakir]][youtube-url-jakir]

### Have a good day, stay with me
<!-- Personal profile -->

[linkedin-shield-jakir]: https://img.shields.io/badge/linkedin-%230077B5.svg?style=for-the-badge&logo=linkedin&logoColor=white
[linkedin-url-jakir]: https://www.linkedin.com/in/jakir-ruet/
[facebook-shield-jakir]: https://img.shields.io/badge/Facebook-%231877F2.svg?style=for-the-badge&logo=Facebook&logoColor=white
[facebook-url-jakir]: https://www.facebook.com/jakir-ruet/
[youtube-shield-jakir]: https://img.shields.io/badge/YouTube-%23FF0000.svg?style=for-the-badge&logo=YouTube&logoColor=white
[youtube-url-jakir]: https://www.youtube.com/@mjakaria-ruet/featured

<!-- Company profile -->

[linkedin-shield-lapissoft]: https://img.shields.io/badge/linkedin-%230077B5.svg?style=for-the-badge&logo=linkedin&logoColor=white
[linkedin-url-lapissoft]: https://www.linkedin.com/company/lapis-soft/
[facebook-shield-lapissoft]: https://img.shields.io/badge/Facebook-%231877F2.svg?style=for-the-badge&logo=Facebook&logoColor=white
[facebook-url-lapissoft]: https://www.facebook.com/GoLapisSoft/
[youtube-shield-lapissoft]: https://img.shields.io/badge/YouTube-%23FF0000.svg?style=for-the-badge&logo=YouTube&logoColor=white
[youtube-url-lapissoft]: https://www.youtube.com/@LapisSoft/featured


