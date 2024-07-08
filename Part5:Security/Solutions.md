# Tasks solution:

## Task 1 :
```yaml
apiVersion: v1 
kind: Role 
metadata: 
  namespace: blue
  name: dev-user
spec: 
- apiGoups: [""]
  resources: ["pods"]
  resourceNames: ["blue-app", dark-blue-app"]
  verbs: ["get", "watch", "list"]

```
==> By adding dark-blue-app in the resourcesName, the dev-user will be able to get details of this pod.

## Task 2 :
## Task 3:
 ```yaml
apiVersion: v1
kind: ServiceAccount
metadata:
  name: gitops
  namespace: project-1
---
apiVersion: rbac.authorization.k8s.io/v1
kind: Role
metadata:
  creationTimestamp: null
  name: gitops-role
  namespace: project-1
rules:
- apiGroups:
  - ""
  resources:
  - secrets
  - configmaps
  verbs: ["create"]
---
apiVersion: rbac.authorization.k8s.io/v1
kind: RoleBinding
metadata:
  namespace: project-1
  name: gitops-rolebinding
roleRef:
  apiGroup: rbac.authorization.k8s.io
  kind: Role
  name: gitops-role
subjects:
- kind: ServiceAccount
  name: gitops
  namespace: project-1

 ```
## Task 4 :
```yaml

apiVersion: v1
kind: Pod
metadata:
  creationTimestamp: null
  labels:
    run: super-user-pod
  name: super-user-pod
spec:
  containers:
  - image: busybox:1.28
    name: super-user-pod
    command: ["sleep","4800"]
    securityContext:
      capabilities:
        add: ["SYS_TIME"]
    resources: {}
  dnsPolicy: ClusterFirst
  restartPolicy: Always
status: {}
```
