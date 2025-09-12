## Day 51: Execute Rolling Updates in Kubernetes

An application currently running on the Kubernetes cluster employs the nginx web server. The application development team has introduced some recent changes that need deployment. They've crafted an image nginx:1.19 with the latest updates.

Execute a rolling update for this application, integrating the nginx:1.19 image. The deployment is named nginx-deployment.

Ensure all pods are operational post-update.

## Solution:

To Ensure all pods are running with the ne image nginx:1.19, run the following,

```bash
kubectl set image deployment/nginx-deployment nginx-container=nginx:1.19
```

Before executing always verify the deployment and container name.

To verify the change is reflected, run:

```bash
kubectl describe deployment/nginx-deployment
```

This will show the image changes.