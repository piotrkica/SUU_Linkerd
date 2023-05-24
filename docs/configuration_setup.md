Underneath commands are assumed to be executed under RHEL compatible OS, such as Fedora.

Start by enabling some modules, then start some services and stop firewall:

```bash
# Enable necessary modules
sudo modprobe br-netfilter

# Enable IP forwarding
sudo sysctl net.ipv4.ip_forward=1

# Enable bridge-netfilter calls for iptables
sudo sysctl net.bridge.bridge-nf-call-iptables=1

# Enable bridge-netfilter calls for ip6tables
sudo sysctl net.bridge.bridge-nf-call-ip6tables=1

# Start containerd service
sudo systemctl start containerd

# Stop firewalld service
sudo systemctl stop firewalld

```

Clean after prev installations:

```sh
# Clean up previous installations
sudo kubeadm reset
sudo rm /etc/cni/net.d/10-flannel.conflist

# Initialize the Kubernetes cluster with a specific pod network CIDR
sudo kubeadm init --pod-network-cidr=10.244.0.0/16

# Remove existing kubeconfig files
rm ~/.kube/*

```
