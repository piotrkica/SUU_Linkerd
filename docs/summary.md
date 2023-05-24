<img src="https://github.com/piotrkica/SUU_Linkerd/blob/mkdocs/docs/img/plaintext_sniffing.png?raw=true" alt="graph1" height="40" style="vertical-align:top; margin:4px">

<img src="https://github.com/piotrkica/SUU_Linkerd/blob/mkdocs/docs/img/ciphertext_sniffing.png?raw=true" alt="graph1" height="40" style="vertical-align:top; margin:4px">

The project involves setting up a Kubernetes cluster and deploying a service mesh using Linkerd. The initial steps focus on preparing the environment by enabling necessary modules, configuring network settings, and initializing the Kubernetes cluster. Following that, the installation and configuration of Linkerd are performed, ensuring the successful deployment of the service mesh.

Once Linkerd is up and running, the project continues by injecting the Linkerd sidecar proxy into the server and client deployments. This enables the service mesh to intercept and manage network traffic for these deployments, providing advanced capabilities such as traffic control, observability, and security. Additionally, the project involves interacting with the deployed pods, including copying files to the server pod, reloading the nginx web server, and performing network analysis using tcpdump and curl commands.

Overall, this project demonstrates the setup and utilization of a service mesh architecture using Linkerd within a Kubernetes cluster. By leveraging Linkerd's powerful features, such as transparent proxy injection, traffic management, and network visibility, the project enhances the communication and management capabilities of the deployed services, contributing to improved reliability, scalability, and observability of the overall system.
