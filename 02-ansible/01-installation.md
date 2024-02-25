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