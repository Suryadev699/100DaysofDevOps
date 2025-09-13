## Day 52: Revert Deployment to Previous Version in Kubernetes

Earlier today, the DevOps team deployed a new release for an application. However, a customer has reported a bug related to this recent release. Consequently, the team aims to revert to the previous version.

There exists a deployment named nginx-deployment; initiate a rollback to the previous revision.

## Solution:

Check the Deployment status first:

```bash
kubectl describe deployment nginx-deployment
```

Now rollout to the previous version using the below command: 

```bash
kubectl rollout undo deployment nginx-deployment
```

You have now successfully initiated the rollback to the previous version.