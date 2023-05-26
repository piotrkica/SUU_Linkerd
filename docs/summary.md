In this demonstration, we compared the communication security between plaintext and ciphertext using Linkerd service mesh. Without the usage of Linkerd, there is a risk of eavesdropping, where a malicious entity could sniff the communication and potentially access sensitive data. However, with the implementation of Linkerd and its proxy sidecars, communication is encrypted using mutual TLS (mTLS), significantly lowering the risk of data exposure.

To showcase the difference, we set up two services, Service A and Service B, within a Kubernetes cluster. We then used ksniff and Wireshark to record the communication between the pods and attempted to observe the data sent. In the first scenario without Linkerd, the communication was in plaintext, making it vulnerable to eavesdropping. However, after deploying Linkerd and enabling encryption, we repeated the experiment. This time, the observed communication was encrypted, making it impossible to read the transmitted data.

The demonstration highlights the importance of using a service mesh like Linkerd to enhance the security of communication within a Kubernetes environment. By leveraging mTLS encryption and proxy sidecars, Linkerd ensures that sensitive data remains protected and inaccessible to unauthorized entities.

### Visible plaintext:

<img src="https://github.com/piotrkica/SUU_Linkerd/blob/mkdocs/docs/img/plaintext_sniffing.png?raw=true" alt="graph1" height="40" style="vertical-align:top; margin:4px">

### Ciphertext:

<img src="https://github.com/piotrkica/SUU_Linkerd/blob/mkdocs/docs/img/ciphertext_sniffing.png?raw=true" alt="graph1" height="40" style="vertical-align:top; margin:4px">
