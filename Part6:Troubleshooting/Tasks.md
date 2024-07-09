## Exercises

1. You are supposed to implement cluster-level logging with a sidecar container.
Create a multi-container Pod named multi. The main application container
named nginx should use the image nginx:1.21.6. The sidecar container named
Exam Essentials
|
153streaming uses the image busybox:1.35.0 and the arguments /bin/sh, -c, and
'tail -n+1 -f /var/log/nginx/access.log'.
2. Define a volume of type emptyDir for the Pod and mount it to the
path /var/log/nginx for both containers.
3. Access the endpoint of the nginx service a couple of times using a wget or curl
command. Inspect the logs of the sidecar container.
4. Create two Pods named stress-1 and stress-2. Define a container that uses
the image polinux/stress:1.0.4 with the command stress and the argu‐
ments /bin/sh, -c, and 'stress --vm 1 --vm-bytes $(shuf -i 20-200 -n
1)M --vm-hang 1'. Set the container memory resource limits and requests to
250Mi.
5. Use the data available through the metrics server to identify which of the Pods,
stress-1 or stress-2, consumes the most memory. Write the name of the Pod
to the file max-memory.txt.
6. Navigate to the directory app-a/ch07/troubleshooting-pod of the checked-out
GitHub repository bmuschko/cka-study-guide. Follow the instructions in the file
instructions.md for troubleshooting a faulty Pod setup.
7. Navigate to the directory app-a/ch07/troubleshooting-deployment of the
checked-out GitHub repository bmuschko/cka-study-guide. Follow the instruc‐
tions in the file instructions.md for troubleshooting a faulty Deployment setup.
8. Navigate to the directory app-a/ch07/troubleshooting-service of the
checked-out GitHub repository bmuschko/cka-study-guide. Follow the instruc‐
tions in the file instructions.md for troubleshooting a faulty Service setup.
9. Navigate to the directory app-a/ch07/troubleshooting-control-plane-node
of the checked-out GitHub repository bmuschko/cka-study-guide. Follow the
instructions in the file instructions.md for troubleshooting a faulty control
plane node setup.
Prerequisite: This exercise requires the installation of the tools Vagrant and
VirtualBox.
10. Navigate to the directory app-a/ch07/troubleshooting-worker-node of the
checked-out GitHub repository bmuschko/cka-study-guide. Follow the instruc‐
tions in the file instructions.md for troubleshooting a faulty worker node setup.