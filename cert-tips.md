```bash
source <(kubectl completion bash) 
echo "source <(kubectl completion bash)" >> ~/.bashrc 
```

# create a pod imperatively
`kubectl run name-pod --image nginx`
with labels option : 
`kubectl run name-pod -l app=MyApp --image nginx `
if you want to expose the Pod through a service of the same name:
` kubectl run name-pod --image=nginx --port=80 --expose` 
# To create a deployment with number of replicas :
`kubectl create deployment --image=nginx name-deployment --replicas=4 --dry-run=client -o yaml > nginx-deployment.yaml`

`--dry-run` : By default as soon as the command is run, the resource will be created. If you simply want to test your command, use the --dry-run=client option. This will not create the resource; instead, it tells you whether the resource can be created and if your command is right.

`-o yaml` : This will output the resource definition in YAML format on the screen.

# Create a service
 `kubectl expose pod redis --port=6379 --name redis-service`
 (This cannot specify the node port.)


Or
`kubectl create service clusterip redis --tcp=6379:6379 --dry-run=client -o yaml`
(This will not use the pod labels as selectors.)

