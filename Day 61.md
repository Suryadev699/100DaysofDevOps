## Day 61: Kubernetes Init Containers

There are some applications that need to be deployed on Kubernetes cluster and these apps have some pre-requisites where some configurations need to be changed before deploying the app container. Some of these changes cannot be made inside the images so the DevOps team has come up with a solution to use init containers to perform these tasks during deployment. Below is a sample scenario that the team is going to test first.

a. Create a Deployment named as *ic-deploy-datacenter*.
b. Configure spec as replicas should be *1*, labels app should be *ic-datacenter*, template's metadata lables *app* should be the same *ic-datacenter*.
c. The *initContainers* should be named as *ic-msg-datacenter*, use image *fedora* with *latest* tag and use command *'/bin/bash', '-c' and 'echo Init Done - Welcome to xFusionCorp Industries > /ic/ecommerce'*. The volume mount should be named as *ic-volume-datacenter* and *mount path* should be */ic*.
d. Main container should be named as *ic-main-datacenter*, use image *fedora* with *latest* tag and use command *'/bin/bash', '-c' and 'while true; do cat /ic/ecommerce; sleep 5; done'*. The volume mount should be named as *ic-volume-datacenter* and mount path should be */ic*.
e. Volume to be named as *ic-volume-datacenter* and it should be an *emptyDir* type.

## Solution:
Create a file init.yaml

```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: ic-deploy-devops
spec:
  replicas: 1
  selector:
    matchLabels:
      app: ic-devops
  template:
    metadata:
      labels:
        app: ic-devops
    spec:
      volumes:
        - name: ic-volume-devops
          emptyDir: {}
      initContainers:
        - name: ic-msg-devops
          image: fedora:latest
          command: ['/bin/bash', '-c', 'echo Init Done - Welcome to xFusionCorp Industries > /ic/ecommerce']
          volumeMounts:
            - mountPath: /ic
              name: ic-volume-devops
      containers:
        - name: ic-main-devops
          image: fedora:latest
          command: ['/bin/bash', '-c', 'while true; do cat /ic/ecommerce; sleep 5; done']
          volumeMounts:
            - mountPath: /ic
              name: ic-volume-devops
---
```
Apply the configuration using

```bash
kubectl apply -f init.yaml
```

You have created and deployed init containers in Kubernetes cluster.