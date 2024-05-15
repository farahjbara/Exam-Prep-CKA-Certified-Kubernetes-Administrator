## ETCD BACKUP & RESTORE

ETCD is the key value data store of the Kubernetes Cluster. The data is stored for service discovery and cluster management. It is important to take a backup of the ETCD as a measure against failures. <br>
### BackUp ETCD :
1. First of all, we need to make sure that we have the etcdctl installed. <br>
    `etcdctl version` <br>
**Notes:**
 * etcdctl is a command line client for etcd.<br>
 * For tasks such as back up and restore, we will always use the ETCDCTL_API for version 3. <br>
2. Verify the version of ETCD running in the cluster: 
    ```bash
            
            controlplane ~ ✖ kubectl  get pod -n kube-system 
                            NAME                                   READY   STATUS    RESTARTS   AGE
                            coredns-69f9c977-6zwpj                 1/1     Running   0          6m20s
                            coredns-69f9c977-q6t4s                 1/1     Running   0          6m20s
                       >    etcd-controlplane                      1/1     Running   0          6m36s
                            kube-apiserver-controlplane            1/1     Running   0          6m36s
                            kube-controller-manager-controlplane   1/1     Running   0          6m34s
                            kube-proxy-5wpv6                       1/1     Running   0          6m20s
                            kube-scheduler-controlplane            1/1     Running   0          6m34s
            
                            kubectl describe pod etcd-controlplane -n kube-system | grep  Image:
                         >  Image:         registry.k8s.io/etcd:3.5.10-0
    ```
3. From the same descibe command reveals the configuration of the etcd service : <br>
   `--listen-client-urls` <br>
   `--cert-file=`<br>
   `--key-file=` <br>
   `--trusted-ca-file=` <br>
4. Take a snapshot of the ETCD database using the built-in snapshot functionality:
   ```bash
          controlplane ~ ✖ ETCDCTL_API=3 etcdctl \
                          --endpoints=https://127.0.0.1:2379 \
                          --cacert=/etc/kubernetes/pki/etcd/ca.crt \
                          --cert=/etc/kubernetes/pki/etcd/server.crt \
                          --key=/etc/kubernetes/pki/etcd/server.key \
                          snapshot save /opt/snapshot-backup.db
                        > Snapshot saved at /opt/etcd-backup.bd
          
   ```

### Restoring ETCD: 
To store etcd from backup , we use the etcdctl snapshot restore command: <br>

    ```bash
          sudo ETCDCTL_API= 3 etcdctl --data-dir=/var/lib/from-backup snapshot restore \opt\etcd-backup
          verify : sudo ls /var/lib/from-backup
                   > member
    ```
    <br>
To terminate , change the value of the attribute spec.volumes.hostPath with name `name: etcd-data` <br>
   path: ~/var/lib/etcd~
   ```bash
           - hostPath:
               path: /var/lib/from-backup 
               type: DirectoryOrCreate
             name: etcd-data
   ```