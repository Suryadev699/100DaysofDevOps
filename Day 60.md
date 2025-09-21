## Day 60: Persistant Volumes in Kubernetes

Create a *PersistentVolume* named as *pv-datacenter*. Configure the *spec* as storage class should be *manual*, set capacity to *3Gi*, set access mode to *ReadWriteOnce*, volume type should be *hostPath* and set path to */mnt/data* (this directory is already created, you might not be able to access it directly, so you need not to worry about it).

Create a *PersistentVolumeClaim* named as *pvc-datacenter*. Configure the spec as storage class should be *manual*, request *2Gi* of the storage, set access mode to *ReadWriteOnce*.

Create a pod named as *pod-datacenter*, mount the persistent volume you created with claim name *pvc-datacenter* at document root of the web server, the container within the pod should be named as *container-datacenter* using image *nginx* with *latest* tag only (remember to mention the tag i.e *nginx:latest*).

Create a *node port* type *service* named *web-datacenter* using node port *30008* to expose the web server running within the pod.

## Solution:

Create a *config.yaml* file:

```yaml
apiVersion: v1
kind: PersistentVolume
metadata:
  name: pv-datacenter
spec:
  capacity:
    storage: 3Gi
  volumeMode: Filesystem
  accessModes:
    - ReadWriteOnce
  persistentVolumeReclaimPolicy: Retain
  storageClassName: manual
  hostPath:
    path: /mnt/data
    type: DirectoryOrCreate
---
apiVersion: v1
kind: PersistentVolumeClaim
metadata:
  name: pvc-datacenter
spec:
  accessModes:
    - ReadWriteOnce
  storageClassName: manual
  resources:
    requests:
      storage: 2Gi
---
apiVersion: v1
kind: Pod
metadata:
  name: pod-datacenter
  labels:
    app: datacenter
spec:
  containers:
  - name: container-datacenter
    image: nginx:latest
    volumeMounts:
    - name: nginx-storage
      mountPath: /var/www/html    # Document root for nginx server
    ports:
    - containerPort: 80
  volumes:
  - name: nginx-storage
    persistentVolumeClaim:
      claimName: pvc-datacenter
---
apiVersion: v1
kind: Service
metadata:
  name: web-datacenter
spec:
  type: NodePort
  ports:
  - port: 80
    nodePort: 30008
    protocol: TCP
  selector:
    app: datacenter
---
```

Apply the above config using

```bash
kubectl apply -f config.yaml
```

You have a Persistant Volume, Persistant Volume Claim, Pod, Service created.
Click the *Website* button on the top right of the terminal to see the contents.