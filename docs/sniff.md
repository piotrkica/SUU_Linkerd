Underneath commands are assumed to be executed under RHEL compatible OS, such
as Fedora.

Start by enabling some modules, then start some services and stop firewall:

*sudo modprobe br-netfilter*
*sudo sysctl net.ipv4.ip_forward=1*
*sudo sysctl net.bridge.bridge-nf-call-iptables=1*
*sudo sysctl net.bridge.bridge-nf-call-ip6tables=1*
*sudo systemctl start containerd*
*sudo systemctl stop firewalld*

Clean after prev installations:

*sudo kubeadm reset*
*sudo rm /etc/cni/net.d/10-flannel.conflist*
*sudo kubeadm init --pod-network-cidr=10.244.0.0/16*
*rm ~/.kube/\**

Install k8s cluster:

*sudo cp /etc/kubernetes/admin.conf ~/.kube/config*
*sudo chown <user>:<user> ~/.kube/config*
*kubectl apply -f <flannel>*
*kubectl taint node fedora.36 node-role.kubernetes.io/control-plane-*
*kubectl taint node fedora.36 node-role.kubernetes.io/master-*

delete loop from config:

*KUBE_EDITOR=vim kubectl -n kube-system edit configmap coredns*
*kubectl -n kube-system rollout restart coredns*

## plaintext

*kubectl create deployment server --image nginx --replicas 1*
*kubectl create deployment client --image nginx --replicas 1*
*kubectl expose deployment server --port 80*
*kubectl expose deployment client --port 80*
*kubectl get pods -l app=client # client-<...>*
*kubectl get pods -l app=server # server-<...>*
*kubectl cp index3.html server-<...>:usr/share/nginx/html/index.html*
*kubectl exec server-<...> -- nginx -s reload*
*kubectl -i -t exec client-<...> -- bash*
*kubectl get services -l app=server  # <cluster-ip-server>*

### searching for server-<...> netnamespace:

find server's IP:

*kubectl get pods -l app=server -o wide*

find and copy netnamespace <server-ns> having <server-ip>:

*sudo ip -all netns exec ip -c a*
*<user>@<localhost>:~$ sudo ip netns exec <server-ns> tcpdump -i eth0 -A port http*
*root@client-<...>:~# curl <cluster-ip-server>*

## ciphertext

mesh application:

*linkerd check --pre*
*linkerd install --crds | kubectl apply -f -*
*linkerd install | kubectl apply -f - # ~50s Running, 70s wszystkie*
*kubectl -n linkerd rollout restart linkerd-destination [opcjonalnie]*
*linkerd check*

ignore warning:

*kubectl get deployment server -o yaml | linkerd inject - | kubectl apply -f -*
*kubectl get deployment client -o yaml | linkerd inject - | kubectl apply -f -*

linkerd is starting new pods, search for new pods and their netnamespaces:
find new client's name:

*kubectl get pods -l app=client*

find new server's name:

*kubectl get pods -l app=client*
*kubectl get pods -l app=server*
*kubectl -c nginx cp index3.html server-<new...>:usr/share/nginx/html/index.html*
*kubectl -c nginx  exec server-<new...> -- nginx -s reload*

write down <server-new-ip>:

*kubectl get pods -l app=server -o wide*

find and copy netnamespace <server-new-ns> having <server-new-ip>:

*sudo ip -all netns exec ip -c a*
*kubectl -c nginx -i -t exec client-<new...> -- bash*
*eng@fedora:~$ sudo ip netns exec <server-new-ns> tcpdump -i eth0 -A port http*
*root@client-<new...>:~# curl <cluster-ip-server>*
