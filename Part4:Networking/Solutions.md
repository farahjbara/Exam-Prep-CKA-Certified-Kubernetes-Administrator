# Tasks Solution :
### Task 1 :

``` yaml
apiVersion: networking.k8s.io/v1
kind: NetworkPolicy
metadata:
  name: webserver-policy
  namespace: webserver
spec:
  podSelector:
    matchLabels:
      role: db
  policyTypes:
  - Ingress
  - Egress
  ingress:
  - from:
    - podSelector: {}
    - namespaceSelector:
        matchLabels:
          kubernetes.io/metadata.name: internal-client
    ports:
    - protocol: TCP
      port: 80
  egress:
    - to:
      - podSelector: {}

      ports:
      - protocol: TCP
        port: 80
```
### Task 2 :
``` yaml
---
apiVersion: networking.k8s.io/v1
kind: NetworkPolicy 
metadata:
  name: vote-policy
  namespace: instavote

spec:
  podSelector:
    matchExpressions:
      role: vote
  policyTypes:
    - Ingress
    - Egress
  ingress:
    - from:
      - podSelector: {}
      ports:
      - protocol: tcp
        port: 80
  egress:
    - to:
      podSelectors:
        matchLabels:
          role: redis
      ports:
      - protocol: TCP
        port: 6379
---

apiVersion: networking.k8s.io/v1
kind: NetworkPolicy
metadata:
  name: redis-policy
  namespace: instavote

spec:
  podSelector:
    matchExpressions:
      role: redis
  policyTypes:
    - Ingress
  ingress:
    - from:
      - podSelector:
          matchExpressions:
            - key: role
              operator: In
              value: [worker,vote]
      ports:
      - protocol: tcp
        port: 6379
                                  
---
apiVersion: networking.k8s.io/v1
kind: NetworkPolicy
metadata:
  name: worker-policy
  namespace: instavote
spec:
  podSelector:
    matchExpressions:
      role: worker
  policyTypes:
    - Egress
  egress:
    - to:  
      - podSelector:
          matchExpressions:
            - key: role
              operator: In
              value: [redis,db]
---
kind: NetworkPolicy
metadata:
  name: db-policy
  namespace: instavote

spec:
  podSelector:
    matchLabels:
      role: db
  policyTypes:
    - Ingress
  ingress:
    - from:
      - podSelector:
          matchExpressions:
            - key: role
              operator: In
              value: [worker,result]
      ports:
      - protocol: tcp
        port: 5342


---
apiVersion: networking.k8s.io/v1
kind: NetworkPolicy
metadata:
  name: result-policy
  namespace: instavote

spec:
  podSelector:
    matchLabels:
      role: result
  policyTypes:
    - Ingress
    - Egress
  ingress:
    - from:
      - podSelector: {}
      ports:
      - protocol: tcp
        port: 80
  egress:
    - to :
      - podSelector: 
          matchLabels:
            role: db

       ports:
       - protocol: TCP
         port: 5342



```
