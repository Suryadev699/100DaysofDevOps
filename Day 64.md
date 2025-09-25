## Day 64: Fix python app on Kubernetes Cluster

One of the DevOps engineers was trying to deploy a python app on Kubernetes cluster. Unfortunately, due to some mis-configuration, the application is not coming up. Please take a look into it and fix the issues. Application should be accessible on the specified nodePort.

The deployment name is *python-deployment-xfusion*, its using *poroko/flask-demo-app* image. The deployment and service of this app is already deployed.
*nodePort* should be *32345* and *targetPort* should be python flask app's default port.

## Solution:

First see the Cluster Status, run

```bash
kubectl get all
```
This will print all the details of the cluster.

Here i have noticed that the pod is in ErrImagePull state. 
Use *kubectl describe* command to check the configuration details.
The image name is mis-configured also the port configuration is not matching.

To make the change, run 

```bash
kubectl edit deployment/python-deployment-xfusion
```
This will open the deployment configuration. Edit the image name, save and exit.

Now, to edit the service, run

```bash
kubectl edit service/python-service-xfusion
```
This opens the service configuration, change the port and target port to the flask app's default port i.e., 5000 and save and exit.

Wait few minutes to pick the changes and click the app button on top right screen. You should see the site up and running.