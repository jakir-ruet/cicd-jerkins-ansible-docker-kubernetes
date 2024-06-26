[![LinkedIn][linkedin-shield-lapissoft]][linkedin-url-lapissoft]
[![Facebook-Page][facebook-shield-lapissoft]][facebook-url-lapissoft]
[![Youtube][youtube-shield-lapissoft]][youtube-url-lapissoft]

## Visit Us [Lapis Soft](http://www.lapissoft.com)

#### The complete DevOps project using Jenkins, Docker, Ansible as well as Kubernetes on AWS.

##### Jenkins Install on EC2 Instance

***Prerequisites***
To follow this tutorial, you will need:
1. Ubuntu Server 22.04 server configured with a non-root user and firewall.
2. OpenJDK 11 or upper

###### Java & OpenJDK Install
##### [Open JDK Install](https://jdk.java.net/21/)

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

###### [Git Install & Configure on Jenkins](./01-jenkins/02-git-installation.md)

###### [Maven Install & Configure on Jenkins](./01-jenkins/03-maven-installation.md)

##### Docker Installation on EC2 Instance

Add Docker's official GPG/GnuPG (GNU Privacy Guard) key:

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

##### Kubernetes Installation on EC2 Instance

#### [AWS CLI Install](https://docs.aws.amazon.com/cli/latest/userguide/getting-started-install.html) on EC2 Instance
```bash
apt install awscli
```

#### [Kubectl Install](https://kubernetes.io/docs/tasks/tools/install-kubectl-linux/#install-kubectl-binary-with-curl-on-linux)
```bash
curl -LO "https://dl.k8s.io/release/$(curl -L -s https://dl.k8s.io/release/stable.txt)/bin/linux/amd64/kubectl"
```
```bash
sudo install -o root -g root -m 0755 kubectl /usr/local/bin/kubectl
```
If you do not have root access on the target system, you can still install kubectl to the ~/.local/bin directory:
```bash
chmod +x kubectl
mkdir -p ~/.local/bin
mv ./kubectl ~/.local/bin/kubectl
# and then append (or prepend) ~/.local/bin to $PATH
```
Check Installation
```bash
kubectl version --client
```

#### [kOps Install](https://kops.sigs.k8s.io/getting_started/install/)

Kubernetes provides excellent container orchestration, but setting up a Kubernetes cluster from scratch can be painful. One solution is to use Kubernetes Operations, or kOps. kOps, also known as Kubernetes operations, is an open-source project which helps you create, destroy, upgrade, and maintain a highly available, production-grade Kubernetes cluster. Depending on the requirement, kOps can also provision cloud infrastructure.

```bash
curl -Lo kops https://github.com/kubernetes/kops/releases/download/$(curl -s https://api.github.com/repos/kubernetes/kops/releases/latest | grep tag_name | cut -d '"' -f 4)/kops-linux-amd64
chmod +x kops
sudo mv kops /usr/local/bin/kops
```
Check Installation
```bash
kops version
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


