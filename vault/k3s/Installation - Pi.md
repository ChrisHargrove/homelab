---
tags:
  - k3s
  - installation
  - hardware
---
[Link](https://docs.k3s.io/installation/requirements)
## Setup Pi
### Install Raspbian into Pi
Install a fresh install of Raspbian onto the Pi, ensure that it is not the graphical version as this machine will be accessed using ssh.
### Clean and update the System and packages
First we need to update all the currently installed system packages.
```bash
sudo apt update && sudo reboot
```

Then remove any unused/un-required packages and then clean the package cache
```bash
sudo apt autoremove && sudo apt clean
```
### Install essential tools
This step will install tools such as curl and wget which will be useful.
```bash
sudo apt install -y curl wget
```
### Setup cgroup
This step is required for the k3s installation to work correctly on a pi5
```bash
sudo nano /boot/firmware/cmdline.txt
```

And the following section needs to be added to the file, or modified to match if they already exist.
```
cgroup_memory=1 cgroup_enable=memory
```
### Reboot
```bash
sudo reboot
```

## Install K3s
### Run the installation script
```bash
curl -sfL https://get.k3s.io | sh -
```
This will install k3s as a server (control plane) and create a single-node Kubernetes cluster.
### Verify installation
```bash
sudo systemctl status k3s
```
If the service is up and running, everything went ok.

## Configure K3s
First check that kubectl commands work, it should have been installed as part of k3s.
```bash
sudo kubectl get nodes
```
We should see the Raspberry Pi listed as one of the nodes as a control plane.
### Add another node (optional)
First we need to get the clusters node token from the existing node.
```bash
sudo cat /var/lib/rancher/k3s/server/node-token
```

Using this token we can attach the new node.
```bash
sudo curl -sfL https://get.k3s.io | K3S_URL=https://<master_node_ip>:6443 K3S_TOKEN=<your_token> sh -
```
Replace the master node ip to be the first node ip address in the local network and the token with the token we got in the previous command.

To verify the new node installation we can run the following.
```bash
sudo kubectl get nodes
```
And we should see the new node as a worker.

## Copy .kube/config from cluster to workstation
Before we can administrate ensure that you have the kubernetes command line tools installed on the machine that you want to use. In this case its on a Mac so we used homebrew to install it.
For other machines refer to the reference docs: [Here](https://kubernetes.io/docs/tasks/tools/).
```bash
brew install kubectl
```

Now that the machine is all setup and running now we need to administrate it. This can be done on the machine or we can do it from another machine.

To do this we need the .kube/config file. When using k3s this can be found at location:
```
/etc/rancher/k3s/k3s.yaml
```

Top copy it from the remote machine to the machine you wish to administrate from we can use secure copy.
On the administrating machine run the following code:
```bash
scp <host>@<ip-address>:/etc/rancher/k3s/k3s.yaml ~/k3s.yaml
```

> [!tip]
> In the event that this command doesn't work you can cat the file to the terminal and create it manually on your machine.

We now need to edit this file so that it points to the remote machine, this is done by editing the server to point at one of the master nodes in the cluster.
```yaml
clusters:
- cluster:
    server: https://<pi-ip-address>:6443
```

Now we can merge this configuration into our main kube-config file so that we can select which cluster we want to administrate.
```bash
cp ~/.kube/config ~/.kube/config.bak

KUBECONFIG=~/.kube/config:~/k3s.yaml kubectl config view --flatten > ~/.kube/config.new

mv ~/.kube/config.new ~/.kube/config
```
This command will backup the previous file before making any changes then it will merge the current config and the newly acquired one into a single file and then place it in the correct place.
