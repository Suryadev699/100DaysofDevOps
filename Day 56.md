## Day 56: Deploy Nginx Web Server on Kubernetes Cluster

Some of the developers are developing a static website and they want to deploy it on Kubernetes cluster. They want it to be highly available and scalable. Therefore, based on the requirements, the DevOps team has decided to create a deployment for it with multiple replicas. Below you can find more details about it:

Create a *deployment* using *nginx* image with *latest* tag only and remember to mention the tag i.e *nginx:latest*. Name it as *nginx-deployment*. The container should be named as *nginx-container*, also make sure *replica* counts are *3*.

Create a *NodePort* type service named *nginx-service*. The nodePort should be *30011*.

## Solution:

Create a *nginx-deployment.yaml* file

```yaml
---
apiVersion: apps/v1
kind: Deployment
metadata:
  name: nginx-deployment
spec:
  replicas: 3
  selector:
    matchLabels:
      app: nginx
  template:
    metadata:
      labels:
        app: nginx
    spec:
      containers:
      - name: nginx-container
        image: nginx:latest
        ports:
        - containerPort: 80
---
apiVersion: v1
kind: Service
metadata:
  name: nginx-service
spec:
  type: NodePort
  selector:
    app: nginx
  ports:
  - port: 80
    targetPort: 80
    nodePort: 30011
---
```

Apply the configuration using the command,

```bash
kubectl apply -f nginx-deployment.yaml
```

You now have created the nginx deployment and the site is accessible via the port 30011.