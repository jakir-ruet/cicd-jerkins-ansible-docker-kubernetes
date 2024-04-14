##### Kubernetes Installation on EC2 Instance

***Prerequisites***
To follow this tutorial, you will need:
1. Ubuntu Server 22.04 server configured with a non-root user and firewall.

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