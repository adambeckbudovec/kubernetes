# kubernetes

Setting up Kubernetes involves several steps, including installing and configuring components. Below are detailed instructions for installing Kubernetes on Ubuntu and CentOS, along with some OS-specific prerequisites.

### Kubernetes Installation on Ubuntu:

#### Prerequisites:

1. **System Requirements:**
   - Ensure that your system meets the hardware requirements for Kubernetes.
   - Make sure your system is running a supported version of Ubuntu.

2. **Docker Installation:**
   - Kubernetes relies on containerization, and Docker is a common container runtime. Install Docker on your Ubuntu system:

     ```bash
     sudo apt update
     sudo apt install -y docker.io
     ```

     Start and enable the Docker service:

     ```bash
     sudo systemctl start docker
     sudo systemctl enable docker
     ```

#### Kubernetes Installation:

1. **Install Kubernetes Components:**

   Install kubeadm, kubelet, and kubectl:

   ```bash
   sudo apt update
   sudo apt install -y kubelet kubeadm kubectl
   ```

2. **Initialize the Kubernetes Cluster:**

   On the master node, initialize the cluster:

   ```bash
   sudo kubeadm init --pod-network-cidr=192.168.0.0/16
   ```

   Follow the instructions provided by kubeadm to set up your kubeconfig:

   ```bash
   mkdir -p $HOME/.kube
   sudo cp -i /etc/kubernetes/admin.conf $HOME/.kube/config
   sudo chown $(id -u):$(id -g) $HOME/.kube/config
   ```

3. **Install a Pod Network Addon:**

   Choose a pod network addon, such as Calico or Flannel, and install it:

   - Calico:

     ```bash
     kubectl apply -f https://docs.projectcalico.org/v3.18/manifests/calico.yaml
     ```

   - Flannel:

     ```bash
     kubectl apply -f https://raw.githubusercontent.com/coreos/flannel/master/Documentation/kube-flannel.yml
     ```

4. **Join Worker Nodes:**

   On each worker node, run the `kubeadm join` command provided by `kubeadm init` on the master node.

   ```bash
   sudo kubeadm join <MASTER_NODE_IP>:<MASTER_NODE_PORT> --token <TOKEN> --discovery-token-ca-cert-hash <CERT_HASH>
   ```

   Replace `<MASTER_NODE_IP>`, `<MASTER_NODE_PORT>`, `<TOKEN>`, and `<CERT_HASH>` with the values from the `kubeadm init` output.

### Kubernetes Installation on CentOS:

#### Prerequisites:

1. **System Requirements:**
   - Ensure that your system meets the hardware requirements for Kubernetes.
   - Make sure your system is running a supported version of CentOS.

2. **Docker Installation:**
   - Install Docker on your CentOS system:

     ```bash
     sudo yum install -y docker
     ```

     Start and enable the Docker service:

     ```bash
     sudo systemctl start docker
     sudo systemctl enable docker
     ```

#### Kubernetes Installation:

1. **Install Kubernetes Components:**

   Add the Kubernetes repository and install kubeadm, kubelet, and kubectl:

   ```bash
   sudo yum install -y epel-release
   sudo yum install -y kubeadm kubelet kubectl
   ```

2. **Initialize the Kubernetes Cluster:**

   On the master node, initialize the cluster:

   ```bash
   sudo kubeadm init --pod-network-cidr=192.168.0.0/16
   ```

   Follow the instructions provided by kubeadm to set up your kubeconfig:

   ```bash
   mkdir -p $HOME/.kube
   sudo cp -i /etc/kubernetes/admin.conf $HOME/.kube/config
   sudo chown $(id -u):$(id -g) $HOME/.kube/config
   ```

3. **Install a Pod Network Addon:**

   Choose a pod network addon, such as Calico or Flannel, and install it:

   - Calico:

     ```bash
     kubectl apply -f https://docs.projectcalico.org/v3.18/manifests/calico.yaml
     ```

   - Flannel:

     ```bash
     kubectl apply -f https://raw.githubusercontent.com/coreos/flannel/master/Documentation/kube-flannel.yml
     ```

4. **Join Worker Nodes:**

   On each worker node, run the `kubeadm join` command provided by `kubeadm init` on the master node.

   ```bash
   sudo kubeadm join <MASTER_NODE_IP>:<MASTER_NODE_PORT> --token <TOKEN> --discovery-token-ca-cert-hash <CERT_HASH>
   ```

   Replace `<MASTER_NODE_IP>`, `<MASTER_NODE_PORT>`, `<TOKEN>`, and `<CERT_HASH>` with the values from the `kubeadm init` output.

### Verify Kubernetes Cluster:

Check the status of the nodes on the master node:

```bash
kubectl get nodes
```

This should display the master node and any joined worker nodes.

Congratulations! You've successfully installed and configured a Kubernetes cluster on your Ubuntu or CentOS system.
