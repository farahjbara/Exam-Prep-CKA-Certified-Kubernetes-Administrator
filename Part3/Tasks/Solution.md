# Solution:
1- Creating a new file named logs-pv.yaml. The contents could look as follows:
``` yaml
apiVersion:
kind: PersistentVolume
metadata:
    name: logs-pv
spec:
  capacity:
    storage: 2Gi
  accessModes:
  - ReadWriteOnce 
  - ReadOnlyMany
  persistentVolumeReclaimPolicy: Delete
  hostPath:
    path: /tmp/logs
  storageClass: ""
```
2- Creating another file named logs-pvc to bound logs-pv :
``` yaml
apiVersion:
kind: PersistentVolumeClaim
metadata:
    name: logs-pv
spec:
  resources:
    request:
      storage: 1Gi
  accessModes:
    - ReadWriteOnce 
  storageClassName: ""  
```
3- Mounting PersistentVolumeClaims in nginx pod :
``` yaml
apiVersion: v1
kind: Pod 
metadata:
  name: nginx-pod  
spec:
  volumes:
    - name: logs-nginx
      persistentVolumeClaim:
        claimName: nginx-pv

  containers:
  - name: nginx
    image: nginx:latest
    volumeMounts:
      - name: logs-nginx
        mountPath: /var/log/nginx
```
4-  `kubectl exec nging-pod -it -- bin/sh` <br>
     ># cd /var/log/nginx <br>
     ># touch nginx.log <br>
5- `kubectl delete pvc  logs-pvc` <br>
   `kubectl delete pod nginx-pod` <br>
<br>
 What happens to the PersistentVolume?
 <br>
 ==> Delete the Pod and the PersistentVolumeClaim. The PersistentVolume will be
deleted automatically due to its reclaim policy.<br>
6- `kubectl get storageclass` <br>
     > provisioner :  k8s.io/minikube <br>

7- Create the file custom-pvc.yaml to define the PersistentVolumeClaim: <br>
``` yaml
apiVersion: storage.k8s.io/v1
kind: StorageClass
metadata:
  name: custom
provisioner: k8s.io/minikube
```

8- Create the file custom-pvc.yaml to define the PersistentVolumeClaim :
``` yaml
apiVersion: v1
kind: PersistentVolumeClaim
metadata:
  name: custom-pvc
spec:
  resources:
     requests:
        storage:  500Mi
  accessModes:
     - ReadWriteOnce
  storgeClass: custom  

```
9- Write the name of the PersistentVolume to the file pv-name.txt: <br>
  >` $ echo "pvc-9852" > pv-name.txt` <br>
10- Deleting the PersistentVolumeClaim will delete the bound PersistentVolume as well.