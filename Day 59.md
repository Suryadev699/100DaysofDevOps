## Day 59: Toubleshoot Kubernetes Deployment issues

Last week, the DevOps team deployed a *redis app* on Kubernetes cluster, which was working fine so far. This morning one of the team members was *making some changes* in this *existing setup*, but he made some *mistakes* and the *app went down*. We need to *fix* this as soon as possible. Please take a look.

The deployment name is *redis-deployment*. The pods are *not in running state* right now, so please look into the issue and fix the same.

## Solution:

First get all necessary info regarding the cluster

-------------------------------------------------------------------------------
"""
user@jumphost ~$ k get all
NAME                                    READY   STATUS              RESTARTS   AGE
pod/redis-deployment-54cdf4f76d-m4dgw   0/1     ContainerCreating   0          23s

NAME                 TYPE        CLUSTER-IP   EXTERNAL-IP   PORT(S)   AGE
service/kubernetes   ClusterIP   10.96.0.1    <none>        443/TCP   12m

NAME                               READY   UP-TO-DATE   AVAILABLE   AGE
deployment.apps/redis-deployment   0/1     1            0           23s

NAME                                          DESIRED   CURRENT   READY   AGE
replicaset.apps/redis-deployment-54cdf4f76d   1         1         0       23s
"""

-------------------------------------------------------------------------------

Here we observe that the redis pod is stuck in *ContainerCreating* state. 
Use *kubectl describe* command to check the configuration details.

-------------------------------------------------------------------------------
"""
user@jumphost ~$ k describe pod/redis-deployment-54cdf4f76d-m4dgw 
Name:             redis-deployment-54cdf4f76d-m4dgw
................
..............
..............
............
...........
..............
Containers:
  redis-container:
    Container ID:   
    Image:          redis:alpin
    Image ID:       
    Port:           6379/TCP
    Host Port:      0/TCP
    State:          Waiting
      Reason:       ContainerCreating
    Ready:          False
    Restart Count:  0
    Requests:
      cpu:        300m
    Environment:  <none>
    Mounts:
      /redis-master from config (rw)
      /redis-master-data from data (rw)
      /var/run/secrets/kubernetes.io/serviceaccount from kube-api-access-n29br (ro)
.........................
......................
.................
.........................
Volumes:
  data:
    Type:       EmptyDir (a temporary directory that shares a pod's lifetime)
    Medium:     
    SizeLimit:  <unset>
  config:
    Type:      ConfigMap (a volume populated by a ConfigMap)
    Name:      redis-conig
    Optional:  false
.........................
....................
................
Events:
  Type     Reason       Age                 From               Message
  ----     ------       ----                ----               -------
  Normal   Scheduled    116s                default-scheduler  Successfully assigned default/redis-deployment-54cdf4f76d-m4dgw to control-plane
  Warning  FailedMount  52s (x8 over 115s)  kubelet            MountVolume.SetUp failed for volume "config" : configmap "redis-conig" not found
"""
-------------------------------------------------------------------------------

Here we observe that the *image name* and *config map* name is mispelled.
To see the configmap details execute the following command

-------------------------------------------------------------------------------
"""
user@jumphost ~$ k get configmap
NAME               DATA   AGE
kube-root-ca.crt   1      14m
redis-config       2      2m17s
"""
-------------------------------------------------------------------------------

Edit the deployment to fix the issue.

-------------------------------------------------------------------------------
"""
user@jumphost ~$ kubectl edit deployments.apps redis-deployment 
.............................
    spec:
      containers:
      - image: redis:alpine
        imagePullPolicy: IfNotPresent
        name: redis-container
.............................
      volumes:
      - emptyDir: {}
        name: data
      - configMap:
          defaultMode: 420
          name: redis-config
        name: config
"""
-------------------------------------------------------------------------------

Save and exit.
You will see a output *deployment.apps/redis-deployment edited*
After few mins check the cluster status

-------------------------------------------------------------------------------
"""
user@jumphost ~$ k get all
NAME                                    READY   STATUS    RESTARTS   AGE
pod/redis-deployment-7c8d4f6ddf-v2zx4   1/1     Running   0          75s

NAME                 TYPE        CLUSTER-IP   EXTERNAL-IP   PORT(S)   AGE
service/kubernetes   ClusterIP   10.96.0.1    <none>        443/TCP   19m

NAME                               READY   UP-TO-DATE   AVAILABLE   AGE
deployment.apps/redis-deployment   1/1     1            1           7m2s

NAME                                          DESIRED   CURRENT   READY   AGE
replicaset.apps/redis-deployment-7c8d4f6ddf   1         1         1       75s
"""
-------------------------------------------------------------------------------

You have fixed the redis-deployment issue.