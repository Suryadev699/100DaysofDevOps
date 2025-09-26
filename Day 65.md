## Day G5: Deploy Redis deployment on Kubernetes

The application development team observed some performance issues with one of the application that is deployed in Kubernetes cluster. After looking into number of factors, the team has suggested to use some in-memory caching utility for DB service. After number of discussions, they have decided to use Redis. Initially they would like to deploy Redis on kubernetes cluster for testing and later they will move it to production. Please find below more details about the task:

Create a *redis deployment* with following parameters:
a. Create a *config map* called *my-redis-config* having *maxmemory 2mb* in *redis-config*.
b. Name of the *deployment* should be *redis-deployment*, it should use *redis:alpine* image and *container name* should be *redis-container*.
c. Also make sure it has only *1* replica.
d. The container should request for *1* CPU.
e. Mount 2 volumes:
    a. An *Empty directory* volume called *data* at path */redis-master-data*.
    b. A *configmap volume* called *redis-config* at path */redis-master*.
    c. The container should expose the *port 6379*.

Finally, redis-deployment should be in an up and running state.

## Solution:

Create a yaml file named redis-deployment.yaml with the following content

```yaml
---
apiVersion: apps/v1
kind: Deployment
metadata:
  name: redis-deployment
spec:
  replicas: 1
  selector:
    matchLabels:
      app: redis
  template:
    metadata:
      labels:
        app: redis
    spec:
      containers:
      - name: redis-container
        image: redis:alpine
        resources:
          requests:
            cpu: 1
        volumeMounts:
        - name: data
          mountPath: /redis-master-data
        - name: config
          mountPath: /redis-master
        ports:
        - containerPort: 6379
      volumes:
      - name: data
        emptyDir: {}
      - name: config
        configMap:
          name: my-redis-config
---
apiVersion: v1
kind: ConfigMap
metadata:
  name: my-redis-config
data:
  redis.conf: maxmemory 2mb
---
```

Apply the config using the command,

```bash
kubectl apply -f redis-deployment.yaml
```

You now have a redis deployment running in your Kubernetes cluster!