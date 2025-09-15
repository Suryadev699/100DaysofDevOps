We are working on an application that will be deployed on multiple containers within a pod on Kubernetes cluster. There is a requirement to share a volume among the containers to save some temporary data. The Nautilus DevOps team is developing a similar template to replicate the scenario. Below you can find more details about it.

Create a pod named *volume-share-datacenter*.

For the first container, use image *fedora* with latest tag only and remember to mention the tag i.e *fedora:latest*, container should be named as *volume-container-datacenter-1*, and run a *sleep* command for it so that it remains in running state. Volume *volume-share* should be mounted at path */tmp/beta.*

For the second container, use image *fedora* with the latest tag only and remember to mention the tag i.e *fedora:latest*, container should be named as *volume-container-datacenter-2*, and again run a *sleep* command for it so that it remains in running state. Volume *volume-share* should be mounted at path */tmp/cluster.*

Volume name should be *volume-share* of type *emptyDir*.

After creating the pod, exec into the first container i.e *volume-container-datacenter-1*, and just for testing create a file *beta.txt* with any content under the mounted path of first container i.e */tmp/beta*.

The file *beta.txt* should be present under the mounted path */tmp/cluster* on the second container *volume-container-datacenter-2* as well, since they are using a shared volume.

## Soution:

Create a *pod.yaml* file:

```yaml
---
apiVersion: v1
kind: Pod
metadata:
  name: volume-share-datacenter
spec:
  containers:
  - name: volume-container-datacenter-1
    image: fedora:latest
    command:
    - sleep
    volumeMounts:
    - name: volume-share
      mountPath: /tmp/beta
  - name: volume-container-datacenter-2
    image: fedora:latest
    command:
    - sleep
    volumeMounts:
    - name: volume-share
      mountPath: /tmp/cluster
  volumes:
  - name: volume-share
    emptyDir: {}
---
```
Apply this configuration using
```bash
kubectl apply -f pod.yaml
```

Verify the pod is running state.
```bash
kubectl get pods
```

Now log into one of the container and create sample file named *beta.txt* with any content. to do this

```bash
kubectl exec -it volume-share-datacenter -c volume-container-datacenter-1 -- /bin/bash

#This will log into the container

"root@volume-container-datacenter-1:/$" echo "Hi" > /tmp/beta/beta.txt

#This will create a dummy text file
#Now exit and log into container 2 to verify the file present, since they are using the same volume-share

kubectl exec -it volume-share-datacenter -c volume-container-datacenter-2 -- /bin/bash

"root@volume-container-datacenter-2:/$" cat /tmp/cluster/beta.txt #This should output the text "Hi"
```

You have now successfully created the Shared Volume in Kubernetes.