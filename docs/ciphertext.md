# Ciphertext

Mesh application:

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

Ignore warning:

```bash
# Ignore warning by injecting Linkerd sidecar into server deployment YAML and applying the modified configuration
kubectl get deployment server -o yaml | linkerd inject - | kubectl apply -f -

# Ignore warning by injecting Linkerd sidecar into client deployment YAML and applying the modified configuration
kubectl get deployment client -o yaml | linkerd inject - | kubectl apply -f -

```

Linkerd is starting new pods, search for new pods and their netnamespaces: find new client's name:

```bash
# Find the name of the new client pod
kubectl get pods -l app=client
```

Find new server's name:

```bash
# Find the names of the new client and server pods
kubectl get pods -l app=client
kubectl get pods -l app=server

# Copy the index3.html file to the server pod's nginx HTML directory using the nginx container
kubectl -c nginx cp index3.html server-<new...>:usr/share/nginx/html/index.html

# Execute the nginx reload command on the server pod using the nginx container
kubectl -c nginx exec server-<new...> -- nginx -s reload
```

Write down **\`<server-new-ip>`**:

```bash
# Write down the IP address of the server pod
kubectl get pods -l app=server -o wide
```

Find and copy netnamespace **`<server-new-ns>`** having **\`<server-new-ip>`**:

```bash
# Find and copy the netnamespace <server-new-ns> corresponding to <server-new-ip>
sudo ip -all netns exec ip -c a
kubectl -c nginx -i -t exec client-<new...> -- bash
eng@fedora:~$ sudo ip netns exec <server-new-ns> tcpdump -i eth0 -A port http
root@client-<new...>:~# curl <cluster-ip-server>
```

<img src="https://github.com/piotrkica/SUU_Linkerd/blob/mkdocs/docs/img/ciphertext_sniffing.png?raw=true" alt="graph1" height="40" style="vertical-align:top; margin:4px">
