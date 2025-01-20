# Updating a Kubernetes Cluster with kubeadm

## Preparing for the Update

1. Verify and update the Kubernetes package repository:

   Edit the repository file:
   ```bash
   sudo vi /etc/apt/sources.list.d/kubernetes.list
   ```

   Update the version in the URL to the next available minor release:

   From:
   ```
   deb [signed-by=/etc/apt/keyrings/kubernetes-apt-keyring.gpg] https://pkgs.k8s.io/core:/stable:/v1.30/deb/ /
   ```
   To:
   ```
   deb [signed-by=/etc/apt/keyrings/kubernetes-apt-keyring.gpg] https://pkgs.k8s.io/core:/stable:/v1.31/deb/ /
   ```

## Upgrading the Master Node

1. Update kubeadm on the master node:
   ```bash
   sudo apt-mark unhold kubeadm
   sudo apt-get update
   sudo apt-get install -y kubeadm=1.31.1-1.1
   sudo apt-mark hold kubeadm
   ```

2. Verify the upgrade plan:
   ```bash
   sudo kubeadm upgrade plan
   ```

3. Upgrade the control plane:
   ```bash
   sudo kubeadm upgrade apply v1.31.1
   ```

4. Prepare the node for maintenance:
   ```bash
   kubectl drain master --ignore-daemonsets
   ```

5. Upgrade kubelet and kubectl:
   ```bash
   sudo apt-mark unhold kubelet kubectl
   sudo apt-get update
   sudo apt-get install -y kubelet=1.31.1-1.1 kubectl=1.31.1-1.1
   sudo apt-mark hold kubelet kubectl
   ```

6. Restart the kubelet:
   ```bash
   sudo systemctl daemon-reload
   sudo systemctl restart kubelet
   ```

7. Make the node available again:
   ```bash
   kubectl uncordon master
   ```

## Upgrading Worker Nodes

For each worker node, perform the following steps:

1. Upgrade kubeadm:
   ```bash
   sudo apt-mark unhold kubeadm
   sudo apt-get update
   sudo apt-get install -y kubeadm=1.31.1-1.1
   sudo apt-mark hold kubeadm
   ```

2. Apply the upgrade:
   ```bash
   sudo kubeadm upgrade node
   ```

3. Prepare the node for maintenance:
   ```bash
   kubectl drain node01 --ignore-daemonsets
   ```

4. Upgrade kubelet and kubectl:
   ```bash
   sudo apt-mark unhold kubelet kubectl
   sudo apt-get update
   sudo apt-get install -y kubelet=1.31.1-1.1 kubectl=1.31.1-1.1
   sudo apt-mark hold kubelet kubectl
   ```

5. Restart the kubelet:
   ```bash
   sudo systemctl daemon-reload
   sudo systemctl restart kubelet
   ```

6. Make the node available again:
   ```bash
   kubectl uncordon node01
   ```

Repeat these steps for each worker node in your cluster.