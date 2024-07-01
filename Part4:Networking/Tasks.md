# Task 1
## Example Exam Task: Create Network Policy
Create a network policy in the namespace webserver named webserver-policy.
<br>
The network policy should allow:
<br>
- TCP using port 80 from pods in the namespace internal-client <br>
- TCP using port 80 from pods in the same namespace
Assign the policy to all pods in the namespace.
<br>
Furthermore, ensure that Pods in the namespace webserver can only connect to other pods in the same namespace using TCP on port 80.
<br>
You can use existing Pods in the namespaces webserver, internal-client and external-client to verify your policy. <br>

# Task 2 
The above network policies are a good start. However you could even further restrict access by creating a granular network policy for each application.
<br>
Create network policies with following specs,
<br>
**vote**
<br>
- allow incoming connections from anywhere, only on port 80
<br>
- allow outgoing connections to redis
block everything else, incoming and outgoing
**redis**
<br>
- allow incoming connections from vote and worker, only on port 6379
- block everything else, incoming and outgoing
<br>
**worker**
<br>
- allow outgoing connections to redis and db
- block everything else, incoming and outgoing
<br>
**db**
<br>
- allow incoming connections from worker and results, only on port 5342
- block everything else, incoming and outgoing
<br>
**result**
<br>
- allow incoming connections from anywhere, only on port 80
<br>
- allow outgoing connections to db
- block everything else, incoming and outgoing