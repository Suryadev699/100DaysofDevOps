## Day 62: Manage Secrets in Kubernetes

The DevOps team is working to deploy some tools in Kubernetes cluster. Some of the tools are licence based so that licence information needs to be stored securely within Kubernetes cluster. Therefore, the team wants to utilize Kubernetes secrets to store those secrets. Below you can find more details about the requirements:

We already have a secret key file *news.txt* under */opt* location on jump host. Create a *generic secret* named *news*, it should contain the *password/license-number* present in *news.txt* file.
Also create a *pod* named *secret-devops*.
Configure pod's *spec* as *container name* should be *secret-container-devops*, image should be *fedora* with *latest* tag (remember to mention the tag with image). Use *sleep* command for container so that it remains in *running* state. Consume the created secret and mount it under */opt/cluster* within the container.
To verify you can exec into the container *secret-container-devops*, to check the secret key under the mounted path */opt/cluster*. Before hitting the Check button please make sure pod/pods are in running state, also validation can take some time to complete so keep patience.


## Solution:

First we need to generate a generic secret key

```bash
kubectl create secret generic news --from-file=password=/opt/news.txt
```

Create a pod config file,

```yaml
apiVersion: v1
kind: Pod
metadata:
  name: secret-xfusion
spec:
  containers:
  - name: secret-container-xfusion
    image: fedora:latest
    resources:
      limits:
        memory: "128Mi"
        cpu: "500m"
    command: ["sleep"] #ensures it is in sleep mode
    args: ["infinity"] #forever
    volumeMounts:
    - name: secret-volume
      mountPath: /opt/apps
  volumes:
  - name: secret-volume
    secret:
      secretName: news
```

Apply this config using

```bash
kubectl apply -f secret-xfusion.yaml
```

You have successfully managed the secrets in kubernetes