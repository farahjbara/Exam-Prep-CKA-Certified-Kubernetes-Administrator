# Part 4: Networking
In the Certified Kubernetes Administrator (CKA) exam, understanding Services and Networking is crucial, accounting for 20% of the curriculum. 
## Kubernetes Networking Basics
Kubernetes is designed as an operating system for managing the complexities of
distributed data and computing. Workloads can be scheduled on a set of nodes to dis‐
tribute the load. The Kubernetes network model enables networking communication
and needs to fulfill the following requirements:
1.Container-to-container communication: Containers running in a Pod often need
to communicate with each other. Containers within the same Pods can send
Inter Process Communication (IPC) messages, share files, and most often com‐
municate directly through the loopback interface using the localhost hostname.
Because each Pod is assigned a unique virtual IP address, each container in the
same Pod is given that context and shares the same port space.
<br>
2. Pod-to-Pod communication: A Pod needs to be able to reach another Pod run‐
ning on the same or on a different node without Network Address Translation
(NAT). Kubernetes assigns a unique IP address to every Pod upon creation from
the Pod CIDR range of its node. The IP address is ephemeral and therefore
cannot be considered stable over time. Every restart of a Pod leases a new IP
address. It’s recommended to use Pod-to-Service communication over Pod-to-
Pod communication. <br>
3. Pod-to-Service communication: Services expose a single, stable DNS name for
a set of Pods with the capability of load balancing the requests across the Pods.
Traffic to a Service can be received from within the cluster or from the outside.
<br>
4. Node-to-node communication: Nodes registered with a cluster can talk to each
other. Every node is assigned a node IP address.
<br>
The specification for the Kubernetes networking model is called Container Network
Interface (CNI). Network plugins that implement the CNI specification are
widely available and can be configured in a Kubernetes cluster by the administrator.
