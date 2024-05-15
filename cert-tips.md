
#CKA Exam Tips

The Certified Kubernetes Administrator (CKA) exam assesses your ability to manage, maintain, and administer Kubernetes clusters. With a two-hour time limit, efficiency and strategic question selection are paramount. <br>

So this guide gives you some tips that can save you a lot of time during the exam <br>.



## Time-Saving Techniques:

### 1. Create Aliases : 
### 2. Master Imperative Commands : 
   #### 2.1. Create a pod : <br>
       `kubectl run name-pod --image nginx` 
        with labels option : <br>
       `kubectl run name-pod -l app=MyApp --image nginx ` <br>
       if you want to expose the Pod through a service of the same name: <br>
      `kubectl run name-pod --image=nginx --port=80 --expose` <br> 
  #### 2.2. Create a deployment :
      `kubectl create deployment --image=nginx name-deployment --replicas=4 ` <br>

  #### 2.3 Create a service :
     `kubectl expose pod redis --port=6379 --name redis-service` <br>
      (This cannot specify the node port.) <br>
    Or
    `kubectl create service clusterip redis --tcp=6379:6379`
     (This will not use the pod labels as selectors.) <br>
  #### 2.4  Use dry-run option  
    `kubectl create deployment --image=nginx name-deployment --replicas=4 --dry-run=client -o yaml > nginx-deployment.yaml`
  It will render the application and print the output resources that checked on the service side. <br>
  By redirecting the output to a file (e.g., > file.yaml) using the > symbol, you can capture the generated YAML definition. <br>






--dry-run=client -o yaml > nginx-deployment.yaml` <br>
`--dry-run` : By default as soon as the command is run, the resource will be created. If you simply want to test your command, use the --dry-run=client option. This will not create the resource; instead, it tells you whether the resource can be created and if your command is right. <br>

`-o yaml` : This will output the resource definition in YAML format on the screen. <br>

**Practice**, **Practice**, **Practice**: Set up a local Kubernetes cluster (e.g., using Minikube) and experiment with resource management through both declarative and imperative approaches. <br>

<p style="text-align"><br> Good luck 🤞</br></p>