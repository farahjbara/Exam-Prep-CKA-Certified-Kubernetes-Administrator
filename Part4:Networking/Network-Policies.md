# Network Policies in Kubernetes: A Guide to Security and Isolation

In Kubernetes, **Network Policies** are essential for securing workloads by controlling how traffic flows between pods, services, and namespaces. They allow you to define the rules for **ingress** (incoming) and **egress** (outgoing) traffic, ensuring that workloads only accept traffic from specific sources or send traffic to designated destinations.

In this article, we’ll explain how to implement Network Policies using a full-stack application example, consisting of frontend, backend, and database components. For security reasons, we aim to isolate the database while allowing controlled communication between the frontend and backend.

## Types of Isolation in Network Policies
There are two primary types of isolation:
- **Ingress Isolation**: Controls incoming traffic to pods.
- **Egress Isolation**: Controls outgoing traffic from pods.

Network Policies can restrict traffic based on the source and destination **pods**, **IP addresses**, **ports**, and **protocols**.

## Example Scenario
In this scenario, we have:
- A **frontend** application running in the `frontend` namespace.
- A **backend** application in the `backend` namespace.
- A **database** in the `db` namespace, which we want to isolate from all other pods except the backend for security reasons.

Our goal is to:
1. Allow traffic from the **frontend** namespace only to the **backend** namespace.
2. Allow traffic from the **backend** namespace only to the **db** namespace.
3. Prevent the **frontend** from directly accessing the **db** namespace, ensuring traffic flows securely through the backend.

## Step 1: Allow Egress Traffic from Frontend to Backend

```yaml
apiVersion: networking.k8s.io/v1
kind: NetworkPolicy
metadata:
  name: frontend
  namespace: frontend
spec:
  podSelector: {}
  policyTypes:
  - Egress
  egress:
  - to:
    - namespaceSelector:
        matchLabels:
          kubernetes.io/metadata.name: backend
    - podSelector:
        matchLabels:
          name: backend
```
In this policy:
<br>
**podSelector: {} **applies the policy to all pods in the frontend namespace.<br>
**The Egress policy **allows outgoing traffic from any pod in the frontend namespace to pods in the backend namespace that have the label name: backend.
## Step 2: Allow Backend Ingress and Egress Traffic
```yaml
apiVersion: networking.k8s.io/v1
kind: NetworkPolicy
metadata:
  name: backend
  namespace: backend
spec:
  podSelector:
    matchLabels:
      name: backend
  policyTypes:
  - Ingress
  - Egress
  ingress:
  - from:
    - namespaceSelector:
        matchLabels:
          kubernetes.io/metadata.name: frontend
    - podSelector:
        matchLabels:
          name: frontend
  egress:
  - to:
    - namespaceSelector:
        matchLabels:
          kubernetes.io/metadata.name: db
    - podSelector:
        matchLabels:
          name: db

```
In this policy:
<br>
**Ingress** allows incoming traffic to the backend pods from pods in the frontend namespace.
<br>
**Egress** allows outgoing traffic from backend pods to the db namespace, specifically to pods with the label name: db.
## Step 3: Allow DNS Egress Traffic for Frontend and Backend
<br>
Sometimes, we need to ensure that pods can resolve DNS queries. The following policies allow egress traffic from the frontend and backend namespaces to the CoreDNS service, ensuring they can resolve domain names.

```yaml
kind: NetworkPolicy
apiVersion: networking.k8s.io/v1
metadata:
  name: all-pods-egress-allow-dns
  namespace: frontend
spec:
  podSelector: {}
  policyTypes:
  - Egress
  egress:
  - to:
    - namespaceSelector:
        matchLabels:
          contains: coredns
      podSelector:
        matchLabels:
          k8s-app: kube-dns
---

kind: NetworkPolicy
apiVersion: networking.k8s.io/v1
metadata:
  name: all-pods-egress-allow-dns
  namespace: backend
spec:
  podSelector: {}
  policyTypes:
  - Egress
  egress:
  - to:
    - namespaceSelector:
        matchLabels:
          contains: coredns
      podSelector:
        matchLabels:
          k8s-app: kube-dns
```

These policies ensure that both frontend and backend pods can communicate with the DNS service in the cluster.
<br>
## Step 4: Verifying the Policies
You can test the policies by checking access between the namespaces. For instance:
<br>
To test access from the frontend to the backend:
<br>
`kubectl exec frontend-pod -n frontend -- curl backend-svc.backend`
<br>
To test access from the frontend to the db (which should be denied):

<br>
`kubectl exec frontend-pod -n frontend -- curl db-svc.db`
<br>
You can also verify that traffic between frontend and db is denied:


`kubectl -n backend exec backend-pod -- curl frontend-svc.frontend ## Should be denied`
<br>
`kubectl -n db exec db-pod -- curl backend-svc.backend ## Should be denied`
<br>
## HOW ALLOW traffic only to a port of an application
<br>
This NetworkPolicy lets you define ingress rules for specific ports of an application. If you do not specify a port in the ingress rules, the rule applies to all ports.
<br>
 **Example**
<br> 

```yaml
kind: NetworkPolicy
apiVersion: networking.k8s.io/v1
metadata:
  name: allow-80
spec:
  podSelector:{}
  ingress:
  - ports:
    - port: 80
      protocol: TCP
   
```
==> This  allow incoming traffic on port 80 from all pods in the same namespace

## Conclusion
Kubernetes Network Policies are a powerful tool for securing and isolating applications running within the cluster. By applying policies at the namespace level, you can control ingress and egress traffic, ensuring that services can only communicate with the components they need to, and reducing the attack surface for potential threats.
<br>
This example demonstrated how to implement Network Policies to isolate traffic in a typical full-stack application. You can further customize these policies to suit the specific needs of your infrastructure.
