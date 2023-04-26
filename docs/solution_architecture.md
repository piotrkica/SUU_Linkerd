## Linkerd

Linkerd is a service mesh for Kubernetes clusters. It is made out of two planes:

1. Data plane - the proxies [@linkerd:proxy], deployed alongside application code. These proxies handle the communication between the microservices and also act as a point at which the service mesh features can be introduced. Written in Rust. These proxies automatically handle all TCP traffic to and from the service, and communicate with the control plane for configuration.

2. Control plane - is a set of services that and provide control over Linkerd as a whole. Written in Go.

inkerd also provides a CLI that can be used to interact with the control and data planes.

<img src="https://linkerd.io/images/architecture/control-plane.png" alt="graph1" height="40" style="vertical-align:top; margin:4px">

### Control plane
The Linkerd control plane is a set of services that run in a dedicated Kubernetes namespace (linkerd by default). The control plane has several components, enumerated below.

1. The destination service
The destination service is used by the data plane proxies to determine various aspects of their behavior. It is used to fetch service discovery information (i.e. where to send a particular request and the TLS identity expected on the other end); to fetch policy information about which types of requests are allowed; to fetch service profile information used to inform per-route metrics, retries, and timeouts; and more.

2. The identity service
The identity service acts as a TLS Certificate Authority that accepts CSRs from proxies and returns signed certificates. These certificates are issued at proxy initialization time and are used for proxy-to-proxy connections to implement mTLS.

3. The proxy injector
The proxy injector is a Kubernetes admission controller that receives a webhook request every time a pod is created. This injector inspects resources for a Linkerd-specific annotation (linkerd.io/inject: enabled). When that annotation exists, the injector mutates the pod’s specification and adds the proxy-init and linkerd-proxy containers to the pod, along with the relevant start-time configuration.

### Data plane
The Linkerd data plane comprises ultralight micro-proxies which are deployed as sidecar containers inside application pods. These proxies transparently intercept TCP connections to and from each pod, thanks to iptables rules put in place by the linkerd-init (or, alternatively, by Linkerd’s CNI plugin).

### Proxy
The Linkerd2-proxy is an ultralight, transparent micro-proxy written in Rust. Linkerd2-proxy is designed specifically for the service mesh use case and is not designed as a general-purpose proxy.

The proxy’s features include:

- Transparent, zero-config proxying for HTTP, HTTP/2, and arbitrary TCP protocols.
- Automatic Prometheus metrics export for HTTP and TCP traffic.
- Transparent, zero-config WebSocket proxying.
- Automatic, latency-aware, layer-7 load balancing.
- Automatic layer-4 load balancing for non-HTTP traffic.
- Automatic TLS.
- An on-demand diagnostic tap API.

The proxy supports service discovery via DNS and the destination gRPC API.

[@linkerd:architecture]