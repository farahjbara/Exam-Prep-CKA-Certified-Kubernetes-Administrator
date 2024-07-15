# Solution :
1. Start by creating a YAML starting point for the Pod in the file named multi-
container.yaml. The following command creates the file:
`ckubectl run multi --image=nginx:1.21.6 -o yaml --dry-run=client \--restart=Never > multi-container.yaml `
Edit the YAML manifest. Add the sidecar container. The contents of the YAML
file could look as follows:

``` yaml 
apiVersion: v1
kind: Pod
metadata:
name: multi
spec:
containers:
- image: nginx:1.21.6
name: nginx
- image: busybox:1.35.0
name: streaming
args: [/bin/sh, -c, 'tail -n+1 -f /var/log/nginx/access.log']
```
2. Add the volume definition and mount it to both containers. The final YAML
manifest is shown here:

``` yaml
apiVersion: v1
kind: Pod
metadata:
name: multi
spec:
containers:
- image: nginx:1.21.6
name: nginx
volumeMounts:
- name: accesslog
mountPath: /var/log/nginx
- image: busybox:1.35.0
name: streaming
args: [/bin/sh, -c, 'tail -n+1 -f /var/log/nginx/access.log']
volumeMounts:
- name: accesslog
mountPath: /var/log/nginx
volumes:
- name: accesslog
emptyDir: {}
```
Create the Pod by pointing the create command to the YAML file:
` kubectl create -f multi-container.yaml`