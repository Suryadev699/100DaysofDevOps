## Day 49: Deploy Applications with Kubernetes Deployments

The DevOps team is delving into Kubernetes for app management. One team member needs to create a deployment following these details:
Create a deployment named httpd to deploy the application httpd using the image httpd:latest (ensure to specify the tag)

## Solution:

To create a Kubernetes deployment named 'httpd' using the image 'httpd:latest', you can use the following command:

```bash
kubectl create deployment httpd --image=httpd:latest
```

This command will create a deployment in the default namespace.

To verify the deployment, run:

```bash
kubectl get deployments
```
This will list all deployments, including the newly created 'httpd' deployment.