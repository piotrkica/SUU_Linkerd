Install k8s cluster:

```bash
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
