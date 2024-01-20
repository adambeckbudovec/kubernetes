# Kubernetes

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

Certainly! Minikube is a tool that allows you to run Kubernetes clusters locally for development and testing purposes. Below are detailed instructions for installing and using Minikube on Ubuntu and CentOS.

### Minikube Installation on Ubuntu:

#### Prerequisites:

1. **System Requirements:**
   - Ensure that your system meets the hardware requirements for running a virtual machine.
   - Make sure your system is running a supported version of Ubuntu.

2. **KVM (optional):**
   - If you prefer using KVM as a hypervisor for better performance, install it:

     ```bash
     sudo apt update
     sudo apt install -y qemu-kvm libvirt-daemon-system libvirt-clients
     sudo usermod -aG libvirt $(whoami)
     ```

     Log out and log back in to apply the group changes.

3. **kubectl Installation:**
   - Install kubectl, the Kubernetes command-line tool:

     ```bash
     sudo apt update
     sudo apt install -y kubectl
     ```

#### Minikube Installation:

1. **Install Minikube:**

   Download and install the Minikube binary:

   ```bash
   curl -LO https://storage.googleapis.com/minikube/releases/latest/minikube_latest_amd64.deb
   sudo dpkg -i minikube_latest_amd64.deb
   ```

2. **Start Minikube:**

   If you are using KVM:

   ```bash
   minikube start --driver=kvm2
   ```

   If you are not using KVM (using the default driver, which is VirtualBox):

   ```bash
   minikube start
   ```

   This command will download the necessary ISO, create a virtual machine, and start the Minikube cluster.

3. **Verify Minikube Status:**

   Check the status of Minikube:

   ```bash
   minikube status
   ```

   This should display that the cluster and kubectl are correctly configured.

### Minikube Installation on CentOS:

#### Prerequisites:

1. **System Requirements:**
   - Ensure that your system meets the hardware requirements for running a virtual machine.
   - Make sure your system is running a supported version of CentOS.

2. **KVM (optional):**
   - If you prefer using KVM as a hypervisor for better performance, install it:

     ```bash
     sudo yum install -y qemu-kvm libvirt-daemon libvirt-daemon-driver-qemu libvirt-daemon-kvm
     sudo usermod -aG libvirt $(whoami)
     ```

     Log out and log back in to apply the group changes.

3. **kubectl Installation:**
   - Install kubectl, the Kubernetes command-line tool:

     ```bash
     sudo yum install -y kubectl
     ```

#### Minikube Installation:

1. **Install Minikube:**

   Download and install the Minikube binary:

   ```bash
   curl -LO https://storage.googleapis.com/minikube/releases/latest/minikube-latest.x86_64.rpm
   sudo rpm -ivh minikube-latest.x86_64.rpm
   ```

2. **Start Minikube:**

   If you are using KVM:

   ```bash
   minikube start --driver=kvm2
   ```

   If you are not using KVM (using the default driver, which is VirtualBox):

   ```bash
   minikube start
   ```

   This command will download the necessary ISO, create a virtual machine, and start the Minikube cluster.

3. **Verify Minikube Status:**

   Check the status of Minikube:

   ```bash
   minikube status
   ```

   This should display that the cluster and kubectl are correctly configured.

### Using Minikube:

Once Minikube is started, you can interact with the local Kubernetes cluster using kubectl:

```bash
kubectl get nodes
```

This should display the Minikube node as the only node in the cluster.

To open the Kubernetes dashboard:

```bash
minikube dashboard
```

This will open the dashboard in your default web browser.

Congratulations! You've successfully installed and configured Minikube on your Ubuntu or CentOS system. You can now use it to test and develop Kubernetes applications locally.

Setting up a Kubernetes GUI involves deploying the Kubernetes Dashboard. The Kubernetes Dashboard is an official web-based user interface for managing and monitoring Kubernetes clusters. Below are detailed instructions for setting up the Kubernetes Dashboard on a Kubernetes cluster.

### Prerequisites:

1. **Kubernetes Cluster:**
   - Ensure that you have a running Kubernetes cluster. This could be a cluster created using tools like Minikube, kind, or a cluster provisioned on a cloud provider.

2. **kubectl:**
   - Install `kubectl`, the Kubernetes command-line tool, on your local machine.

### Deploying Kubernetes Dashboard:

1. **Deploy Kubernetes Dashboard:**

   Run the following command to deploy the Kubernetes Dashboard:

   ```bash
   kubectl apply -f https://raw.githubusercontent.com/kubernetes/dashboard/v2.4.0/aio/deploy/recommended.yaml
   ```

   This command deploys the necessary resources for the Dashboard, including the Deployment, Service, and RBAC configurations.

2. **Create a ClusterRoleBinding:**

   To access the Dashboard, you need to create a `ClusterRoleBinding`. This step grants the necessary permissions to the default Service Account.

   ```bash
   kubectl create clusterrolebinding dashboard-admin-sa \
     --clusterrole=cluster-admin \
     --serviceaccount=kubernetes-dashboard:default
   ```

3. **Access the Dashboard:**

   - To access the Dashboard using `kubectl proxy`:

     ```bash
     kubectl proxy
     ```

     Open the Dashboard at [http://localhost:8001/api/v1/namespaces/kubernetes-dashboard/services/https:kubernetes-dashboard:/proxy/](http://localhost:8001/api/v1/namespaces/kubernetes-dashboard/services/https:kubernetes-dashboard:/proxy/).

   - To access the Dashboard using a NodePort Service (for testing purposes):

     Deploy a NodePort Service for the Kubernetes Dashboard:

     ```bash
     kubectl apply -f - <<EOF
     apiVersion: v1
     kind: Service
     metadata:
       labels:
         k8s-app: kubernetes-dashboard
       name: kubernetes-dashboard-nodeport
       namespace: kubernetes-dashboard
     spec:
       ports:
       - port: 80
         targetPort: 9090
       selector:
         k8s-app: kubernetes-dashboard
       type: NodePort
     EOF
     ```

     Find the NodePort assigned to the service:

     ```bash
     kubectl get svc kubernetes-dashboard-nodeport -n kubernetes-dashboard
     ```

     Open the Dashboard using the NodePort at `http://<NODE_IP>:<NODE_PORT>`.

4. **Accessing the Dashboard Token:**

   To authenticate to the Dashboard, you need a token. Retrieve the token using the following command:

   ```bash
   kubectl -n kubernetes-dashboard describe secret $(kubectl -n kubernetes-dashboard get secret | grep admin-user | awk '{print $1}')
   ```

   Copy the token and use it to log in to the Dashboard.

### Security Considerations:

The Kubernetes Dashboard is a powerful tool, and its default configuration might not be suitable for production environments. Consider securing access to the Dashboard based on your cluster's security requirements.

Remember to follow best practices for securing your Kubernetes cluster and the Dashboard in a production environment. Ensure that you have proper RBAC configurations and network policies in place.
