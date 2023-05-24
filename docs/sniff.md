Underneath commands are assumed to be executed under RHEL compatible OS, such as Fedora.

Start by enabling some modules, then start some services and stop firewall:

```console
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

Install k8s cluster:

```shell script
# Copy the Kubernetes admin configuration to the current user's kubeconfig
sudo cp /etc/kubernetes/admin.conf ~/.kube/config

# Change ownership of the kubeconfig file to the current user
sudo chown <user>:<user> ~/.kube/config

# Apply the flannel network configuration (replace <flannel> with the appropriate file)
kubectl apply -f <flannel>

# Taint the node to remove control-plane and master roles
kubectl taint node fedora.36 node-role.kubernetes.io/control-plane-
kubectl taint node fedora.36 node-role.kubernetes.io/master-

```

delete loop from config:

```bash
# Edit the CoreDNS configuration map
KUBE_EDITOR=vim kubectl -n kube-system edit configmap coredns

# Restart the CoreDNS pods
kubectl -n kube-system rollout restart coredns
```

## Plaintext

```bash
# Create a deployment for the server with nginx image and 1 replica
kubectl create deployment server --image nginx --replicas 1

# Create a deployment for the client with nginx image and 1 replica
kubectl create deployment client --image nginx --replicas 1

# Expose the server deployment on port 80
kubectl expose deployment server --port 80

# Expose the client deployment on port 80
kubectl expose deployment client --port 80

# Get the pods labeled with app=client
kubectl get pods -l app=client # client-<...>

# Get the pods labeled with app=server
kubectl get pods -l app=server # server-<...>

# Copy the index3.html file to the server pod's nginx HTML directory
kubectl cp index3.html server-<...>:usr/share/nginx/html/index.html

# Execute the nginx reload command on the server pod
kubectl exec server-<...> -- nginx -s reload

# Open an interactive bash session in the client pod
kubectl -i -t exec client-<...> -- bash

# Get services labeled with app=server to find the cluster IP
kubectl get services -l app=server # <cluster-ip-server>

```

#### searching for server-<...> netnamespace:

find server's IP:

```bash
# Find the server pod's IP address
kubectl get pods -l app=server -o wide
```

find and copy netnamespace **\<server-ns>** having **\<server-ip>**:

```bash
# Find and copy the netnamespace <server-ns> corresponding to <server-ip>
sudo ip -all netns exec ip -c a
<user>@<localhost>:~$ sudo ip netns exec <server-ns> tcpdump -i eth0 -A port http
root@client-<...>:~# curl <cluster-ip-server>
```

## Ciphertext

mesh application:

```bash
# Perform pre-checks for Linkerd installation
linkerd check --pre

# Install Linkerd control plane CRDs
linkerd install --crds | kubectl apply -f -

# Install Linkerd control plane components
linkerd install | kubectl apply -f - # Takes approximately 50 seconds to start running, 70 seconds for all components to start

# (Optional) Restart the linkerd-destination pod in the linkerd namespace
kubectl -n linkerd rollout restart linkerd-destination

# Perform post-installation checks for Linkerd
linkerd check

```

ignore warning:

```bash
# Ignore warning by injecting Linkerd sidecar into server deployment YAML and applying the modified configuration
kubectl get deployment server -o yaml | linkerd inject - | kubectl apply -f -

# Ignore warning by injecting Linkerd sidecar into client deployment YAML and applying the modified configuration
kubectl get deployment client -o yaml | linkerd inject - | kubectl apply -f -

```

linkerd is starting new pods, search for new pods and their netnamespaces: find new client's name:

```bash
# Find the name of the new client pod
kubectl get pods -l app=client
```

find new server's name:

```bash
# Find the names of the new client and server pods
kubectl get pods -l app=client
kubectl get pods -l app=server

# Copy the index3.html file to the server pod's nginx HTML directory using the nginx container
kubectl -c nginx cp index3.html server-<new...>:usr/share/nginx/html/index.html

# Execute the nginx reload command on the server pod using the nginx container
kubectl -c nginx exec server-<new...> -- nginx -s reload
```

write down **\<server-new-ip>**:

```bash
# Write down the IP address of the server pod
kubectl get pods -l app=server -o wide
```

find and copy netnamespace **\<server-new-ns>** having **\<server-new-ip>**:

```bash
# Find and copy the netnamespace <server-new-ns> corresponding to <server-new-ip>
sudo ip -all netns exec ip -c a
kubectl -c nginx -i -t exec client-<new...> -- bash
eng@fedora:~$ sudo ip netns exec <server-new-ns> tcpdump -i eth0 -A port http
root@client-<new...>:~# curl <cluster-ip-server>
```
