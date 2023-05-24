# Plaintext sniffing

Plaintext

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

### Searching for server-<...> netnamespace:

Finding server's IP:

```bash
# Find the server pod's IP address
kubectl get pods -l app=server -o wide
```

Find and copy netnamespace **`<server-ns>`** having **`<server-ip>`**:

```bash
# Find and copy the netnamespace <server-ns> corresponding to <server-ip>
sudo ip -all netns exec ip -c a
<user>@<localhost>:~$ sudo ip netns exec <server-ns> tcpdump -i eth0 -A port http
root@client-<...>:~# curl <cluster-ip-server>
```

<img src="https://github.com/piotrkica/SUU_Linkerd/blob/mkdocs/docs/img/plaintext_sniffing.png?raw=true" alt="graph1" height="40" style="vertical-align:top; margin:4px">
