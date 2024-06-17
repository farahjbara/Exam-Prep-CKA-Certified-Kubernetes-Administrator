# Part 3: Storage
Storage represents 10% of the CKA (Certified Kubernetes Administrator) exam. As a Kubernetes administrator, you need to be proficient in creating and managing volumes, as well as using best practices to save data from Kubernetes resources.

## Resources
- [Persistent Volumes](https://kubernetes.io/docs/concepts/storage/persistent-volumes/)
- [Persistent Volumes Claims](https://kubernetes.io/docs/concepts/storage/persistent-volumes/#claims-as-volumes\)
- [Storage Classes](https://kubernetes.io/docs/concepts/storage/storage-classes/)
## Volumes Types : 
- emptyDir: A temporary directory that shares a Pod's lifetime.
- configMap: A way to inject configuration data into Pods.
- secret: Similar to configMap, but intended for confidential data.
- nfs: Network File System, which allows sharing of files across a network.
- hostPath: Mounts a file or directory from the host node’s filesystem into a Pod.
- persistentVolumeClaim: A way for users to request persistent storage.
## A Pod defining and mounting a volume
Here's an example of defining and mounting an emptyDir volume in a Pod:
``` yaml 
apiVersion: v1
kind: Pod
metadata:
name: business-app
spec:
containers:
- image: nginx
name: nginx
volumeMounts:
- mountPath: /var/logs
name: logs-volume
volumes:
- name: logs-volume
emptyDir: {} #Volume type

```
<br>
In this example:
<br>
- `emptyDir` volume is created when the Pod is assigned to a Node and exists as long as that Pod runs on that Node. <br>
- The volume is mounted at /var/logs in the nginx container.

## PersistentVolume object 
A PersistentVolume (PV) is a piece of storage in the cluster that has been provisioned by an administrator. Here is an example:
``` yaml
apiVersion: v1
kind: PersistentVolume 
metadata:
    name: pv-logs
spec:
    capacity:
      storage: 1Gi
    accessModes:    
      - ReadWriteOnce
    hostPath:
      path: /mnt/data
```
Key elements of this definition:

**capacity:** Specifies the size of the volume (e.g., 1Gi). <br>

**hostPath:** Uses a directory or file from the host node's filesystem.  <br>
**accessModes:** Specifies the mode of access. Common values are: <br>
  - ReadWriteOnce (RWO): The volume can be mounted as read-write by a single node.  <br>
  - ReadOnlyMany (ROX): The volume can be mounted as read-only by many nodes.  <br>
  - ReadWriteMany (RWX): The volume can be mounted as read-write by many nodes.  <br>

To see the access mode used by a PersistentVolume : <br>
`kubectl get pv pv-logs -o jsonpath='{.spec.accessModes}'`
**Reclaim Policy**
Determines what happens to a volume when it is released. Possible values are Retain, Recycle, and Delete.

## PersistentVolumeClaim object 
A PersistentVolumeClaim (PVC) is a request for storage by a user. Here is an example:
``` yaml
apiVersion: v1
kind: PersistentVolumeClaim
metadata:
    name: pvc-logs
spec:
    ressources:
        requests:
            storage: 256Mi
    accessModes:    
      - ReadWriteOnce
    storgeClassName: ""
```
**Key elements of this definition:**

- resources.requests.storage: Specifies the desired storage size. <br>
- accessModes: Specifies the desired access mode. <br>
- storageClassName: Specifies which storage class to use. If left empty, the default storage class is used.<br>
**Status of PersistentVolumeClaim:**  <br>
Bound ==> the binding to the PersistentVolume was successful.  <br>
Pending ==> the binding to the PersistentVolume is still in progress. <br>
Released ==> the binding to the PersistentVolume was released. <br>

## Mounting PersistentVolumeClaims in a Pod
Here is an example of how to mount a PersistentVolumeClaim in a Pod:

``` yaml
apiVersion: v1
kind: Pod
metadata:
    name: app-consuming-pvc
spec:
    volumes:
      - name: app-storage
        persistentVolumeClaim:
            claimName: db-pvc
containers:
- image: alpine
  name: app
  command: ["/bin/sh"]
  args: ["-c", "while true; do sleep 60; done;"]
volumeMounts:
- mountPath: "/mnt/data"
  name: app-storage

```
In this example: <br>

The Pod named app-consuming-pvc mounts the PersistentVolumeClaim pvc-logs at /mnt/data in the alpine container.

## StorageClass

A StorageClass is used to provision a PersistentVolume dynamically based on its criteria. Here is an example:


```yaml

apiVersion: storage.k8s.io/v1
kind: StorageClass
metadata:
    name: fast
provisioner: kubernetes.io/gce-pd
parameters:
    type: pd-ssd
    replication-type: regional-pd

```
**Key elements of this definition:

- provisioner: Specifies the volume plugin to use (e.g., kubernetes.io/gce-pd for Google Cloud Engine persistent disk).
- parameters: Specifies additional parameters for the provisioner. For example:
    **type:** The type of storage (e.g., pd-ssd for SSD).
    **replication-type:** The replication type (e.g., regional-pd for regional persistent disk).