# Task1: 
A set of new roles and role-bindings are created in the blue namespace for the dev-user. <br> However, the dev-user is unable to get details of the dark-blue-app pod in the blue namespace. Investigate and fix the issue.
<br>

We have created the required roles and rolebindings, but something seems to be wrong.

# Task2:
Create a new user “moon” in your Kubernetes cluster. The user should use certificate credentials to authenticate to the cluster. 
<br>Configure a Role called “moon” that allows all operations on all resources in the namespace “userns-moon”. <br> Assign the Role to the user.

# Task3:
Create a new ServiceAccount gitops in Namespace project-1.
<br> Create a Role and RoleBinding, both named gitops-role and gitops-rolebinding as well. 
<br>
These should allow the new SA to only create Secrets and ConfigMaps in that Namespace.
