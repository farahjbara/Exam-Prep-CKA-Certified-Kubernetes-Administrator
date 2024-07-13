# Troubleshoot application failure


<br> If our application has an error I'd first start with checking if the Pod was running, and in case of multiple replicas, if there's an issue on all or just a few. One important thing here is to try to find out if the failure is because of something inside the Application, or if there's an error running the Pod, or maybe the Service it is a part of.
<br>
To check if a pod is running we can start with the kubectl get pods command. This outputs the status of the Pod
<br>
We can continue with a kubectl describe pod <pod-name> to further investigate the Pod. Status messages appear here.
<br>
We also have the kubectl logs <pod-name> which outputs the stdout and stderr from the containers in the Pod.
<br>
Note that application logging is very much up to the application and the developers of that application. Kubernetes cannot do any magic stuff inside an application.

<br>