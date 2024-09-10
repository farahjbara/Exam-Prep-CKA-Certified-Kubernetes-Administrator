# Environment Variables in Kubernetes Pods
## ConfigMap
ConfigMaps allow you to store non-confidential data in key-value pairs and make it available to your pods as environment variables.

  ### 1. Creating a ConfigMap (Imperative Way) :
  You can create a ConfigMap imperatively using the following command: <br>

  `kubectl create cm my-config --from-literal=KEY1=value1 --from-literal=KEY2=value2`
 >  Replace my-config with the name of your ConfigMap. <br>
 > Replace **KEY1=value1** and **KEY2=value2** with your key-value pairs.
  ### 2. Injecting ConfigMap into a Pod :
 #### Injecting a Single Environment Variable
You can inject a single environment variable from a ConfigMap into a pod by specifying it in the pod's YAML file:

```yaml
apiVersion: v1
kind: Pod
metadata:
  name: my-pod
spec:
  containers:
  - name: my-container
    image: nginx
    env:
    - name: MY_ENV_VAR
      valueFrom:
        configMapKeyRef:
          name: my-config
          key: KEY1
```

**name:** The name of the environment variable inside the container. <br>
**valueFrom.configMapKeyRef.name:** The name of the ConfigMap. <br>
**key:** The specific key in the ConfigMap whose value you want to inject. 
<br> 

   #### Injecting Multiple Environment Variables:

You can inject all key-value pairs from a ConfigMap into a pod:

```yaml
apiVersion: v1
kind: Pod
metadata:
  name: my-pod
spec:
  containers:
  - name: my-container
    image: nginx
    envFrom:
    - configMapRef:
        name: my-config

```
## Secret
Secrets are used to store sensitive data, such as passwords, tokens, and keys. They are similar to ConfigMaps but are intended for confidential information.


  ### 1. Creating a Secret (Imperative Way) :
  You can create a Secret imperatively using the following command: <br>
  ` kubectl create secret generic credentials --from-literal=USERNAME=myuser --from-literal=PASSWORD=mypassword`
> Replace credentials with the name of your Secret. <br>
> Replace USERNAME=myuser and PASSWORD=mypassword with your key-value pairs. <br> 
  ### 2. Injecting Secret into a Pod:
  #### Injecting a Single Environment Variable :
To inject a single environment variable from a Secret into a pod : <br>
```yaml 
apiVersion: v1
kind: Pod
metadata:
  name: my-pod
spec:
  containers:
  - name: my-container
    image: nginx
    env:
    - name: SECRET_USERNAME
      valueFrom:
        secretKeyRef:
          name: credentials
          key: USERNAME

```
**name:** The name of the environment variable inside the container. <br>
**valueFrom.secretKeyRef.name:** The name of the Secret. <br>
**key:** The specific key in the Secret whose value you want to inject. <br>
#### Injecting Multiple Environment Variables
To inject all key-value pairs from a Secret into a pod:<br>
```yaml
apiVersion: v1
kind: Pod
metadata:
  name: my-pod
spec:
  containers:
  - name: my-container
    image: nginx
    envFrom:
    - secretRef:
        name: credentials

```

