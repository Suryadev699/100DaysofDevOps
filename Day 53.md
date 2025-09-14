## Day 53: Resolving Volume Mounts Issue in Kubernetes

We encountered an issue with our Nginx and PHP-FPM setup on the Kubernetes cluster this morning, which halted its functionality. Investigate and rectify the issue:
The pod name is nginx-phpfpm and configmap name is nginx-config. Identify and fix the problem.
Once resolved, copy /home/thor/index.php file from the jump host to the nginx-container within the nginx document root. After this, you should be able to access the website using Website button on the top bar.

## Solution:

Inspect the pod 

```bash
kubectl describe pod nginx-phpfpm
```

Verify the volume mounts of both the containers. The mount paths from both the containers and also the root path must be same.

Here the path must be set to "/usr/share/nginx/html" which is default for nginx site files.

In my scenario, the PHP container is volume mount path set to "/var/www/html" where as nginx container volume mount path is set to "/usr/share/nginx/html". 

Before that we also need to check for the configmap.

```bash
kubectl edit configmap nginx-config
```

Check the root path here

```bash
# Set nginx to fetch files from root
root /usr/share/nginx/html
```
Save and exit.

Now edit the pod configuration.
Here we cannot use the "kubectl edit" to change the properties as the pod is immutable. 

```bash
kubectl get pod nginx-phpfpm -o yaml > pod.yaml
vi pod.yaml
```

Update the PHP container volume mount path.

```bash
-------------------
name: php-fpm-container
volumeMounts:
  - mountPath: /usr/share/nginx/html    #Edit this line
-------------------
# Incase of nginx-container mismatch
name: nginx-container
volumeMounts:
  - mountPath: /usr/share/nginx/html    #Edit this line
```

Recreate the pod.

```bash
kubectl repalce --force -f pod.yaml
```

Check the status of the pod it should be in running state.

Copy the index.php file to the nginx container.

```bash
kubectl cp index.php nginx-phpfpm:/usr/share/nginx/html/index.php -c nginx-container
```

Click on the website button at the top right of the page. You should now see the deafult php configuration page.
