## Day 48: Deploy pods in Kubernetes Cluster

The DevOps team is diving into Kubernetes for application management. One team member has a task to create a pod according to the details below:
Create a pod named pod-nginx using the nginx image with the latest tag. Ensure to specify the tag as nginx:latest.
Set the app label to nginx_app, and name the container as nginx-container

## Solution:

Create a yaml file named pod-nginx.yaml with the following content:

```yaml
apiVersion: v1
kind: Pod
metadata:
  name: pod-nginx
  labels:
    app: nginx_app
spec:
  containers:
  - name: nginx-container
    image: nginx:latest
    ports:
    - containerPort: 80
```

Apply the configuration using the command:

```bash
kubectl apply -f pod-nginx.yaml
```

You now have an nginx pod running in your Kubernetes cluster!

Verify it using the command:

```bash
kubectl get pods
```
You should see the pod-nginx listed among the running pods.