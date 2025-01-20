# Installing Basic Kubernetes Cluster with Kubeadm

This guide covers the installation of a basic Kubernetes cluster using Kubeadm, Kubelet, and Kubectl (Kubernetes version 1.31).

## Prerequisites

### Step 1: Disable Swap

```bash
sudo swapoff -a
```

### Step 2: Install Container Runtime (Containerd)

#### Enable IPv4 packet forwarding

```bash
cat <<EOF | sudo tee /etc/sysctl.d/k8s.conf
net.ipv4.ip_forward = 1
EOF

sudo sysctl --system
```

#### Install Containerd

```bash
# Add Docker's official GPG key:
sudo apt-get update
sudo apt-get install ca-certificates curl
sudo install -m 0755 -d /etc/apt/keyrings
sudo curl -fsSL https://download.docker.com/linux/ubuntu/gpg -o /etc/apt/keyrings/docker.asc
sudo chmod a+r /etc/apt/keyrings/docker.asc
```

# Add the repository to Apt sources:
echo \
  "deb [arch=$(dpkg --print-architecture) signed-by=/etc/apt/keyrings/docker.asc] https://download.docker.com/linux/ubuntu \
  $(. /etc/os-release && echo "$VERSION_CODENAME") stable" | \
  sudo tee /etc/apt/sources.list.d/docker.list > /dev/null
  sudo apt-get update
```

# Install containerd

```bash
sudo apt-get install containerd.io
```

#### Configure the systemd cgroup driver

```bash
containerd config default | sudo tee /etc/containerd/config.toml

sudo vi /etc/containerd/config.toml 
```

Set the following parameters in the config file:
```toml
SystemdCgroup = true

sandbox_image = "registry.k8s.io/pause:3.10" (for k8s 1.31)
OR
sandbox_image = "registry.k8s.io/pause:3.9" (for k8s 1.30)
OR
sandbox_image = "registry.k8s.io/pause:3.8" (for k8s 1.29)
```

Restart containerd:
```bash
sudo systemctl restart containerd
```

## Install Kubeadm, Kubelet, and Kubectl

```bash
sudo apt-get update 
sudo apt-get install -y apt-transport-https ca-certificates curl gpg

curl -fsSL https://packages.cloud.google.com/apt/doc/apt-key.gpg | sudo gpg --dearmor -o /etc/apt/keyrings/kubernetes-archive-keyring.gpg

echo "deb [signed-by=/etc/apt/keyrings/kubernetes-archive-keyring.gpg] https://apt.kubernetes.io/ kubernetes-xenial main" | sudo tee /etc/apt/sources.list.d/kubernetes.list

sudo apt-get update
sudo apt-get install -y kubelet kubeadm kubectl
sudo apt-mark hold kubelet kubeadm kubectl
```

## Initialize the Control-Plane Node

First, find the IP address of the control plane node:

```bash
ip -4 addr show | grep -P '\d+(\.\d+){3}'
```

Then initialize the control-plane:

```bash
kubeadm init --control-plane-endpoint=10.122.16.2 --pod-network-cidr=172.16.0.0/16
```

After initialization, set up the kubeconfig:

```bash
mkdir -p $HOME/.kube
sudo cp -i /etc/kubernetes/admin.conf $HOME/.kube/config
sudo chown $(id -u):$(id -g) $HOME/.kube/config
```

## Set Up the Network

Apply the Weave Net configuration:

```bash
kubectl apply -f https://reweave.azurewebsites.net/k8s/v1.31/net.yaml
```

Edit the Weave Net DaemonSet to add an environment variable:

```bash
kubectl edit daemonset weave-net -n kube-system
```

Add the following environment variable to the weave-net container:

```yaml
- name: IPALLOC_RANGE
  value: 172.16.0.0/16
```

## Join Worker Nodes

On each worker node, run the join command provided by `kubeadm init`. It will look similar to:

```bash
kubeadm join 10.122.16.2:6443 --token dxwe1g.7qsxffwnu0ckte61 \
	--discovery-token-ca-cert-hash sha256:6e77208ab406e350a2add00b0b55348a270776abd176c054214438a2d060abf3 
```

## Verify the Cluster

Check if all nodes are up and running:

```bash
kubectl get nodes
```

Test by running a sample nginx pod:

```bash
kubectl run nginx --image=nginx
kubectl get pods -o wide
```

Your Kubernetes cluster should now be operational!
