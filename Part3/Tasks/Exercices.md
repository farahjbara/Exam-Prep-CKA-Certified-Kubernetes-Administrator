
<ol>
<li> Create a PersistentVolume named  <i>logs-pv</i> that maps to the hostPath  <i> /tmp/logs  </i>.
The access mode should be <b> ReadWriteOnce </b> and <b> ReadOnlyMany </b>. Provision a stor‐
age capacity of 2Gi. Assign the reclaim policy Delete and an empty string as the
storage class. Ensure that the status of the PersistentVolume shows Available. </li>
<li> Create a PersistentVolumeClaim named  <i> logs-pvc  </i>. The access it uses is
ReadWriteOnce. Request a capacity of <b> 1Gi</b>. Ensure that the status of the Persis‐
tentVolume shows Bound.</li>
<li> Mount the PersistentVolumeClaim in a Pod running the image nginx at the
mount path  <i> /var/log/nginx </i>.</li>
<li> Open an interactive shell to the container and create a new file named  <i> my-
nginx.log</i> in <i> /var/log/nginx </i>. Exit out of the Pod.</li>
<li> Delete the Pod and PersistentVolumeClaim. What happens to the PersistentVo‐
lume? </li>
<li> List the available storage classes and identify the default storage class. Note the
provisioner.</li>
<li> Create a new storage class named custom using the provisioner of the default
storage class.</li>
<li> Create a PersistentVolumeClaim named  <i> custom-pvc </i>. Request a capacity of
 <b> 500Mi</b> and declare the access mode  <b> ReadWriteOnce </b>. Assign the storage class
name custom.</li>
<li> The PersistentVolume should have been provisioned dynamically. Find out the
name and write it to the file named   <i> pv-name.txt</i>.</li>
<li> Delete the PersistentVolumeClaim. What happens to the PersistentVolume?</li>
    
</ol>  
