## Day 66: Deploy MYSQL on Kubernetes Cluster

A new MySQL server needs to be deployed on Kubernetes cluster. The DevOps team was working on to gather the requirements. Recently they were able to finalize the requirements and shared them with the team members to start working on it. Below you can find the details:
Create a PersistentVolume *mysql-pv*, its capacity should be *250Mi*, set other parameters as per your preference.
Create a PersistentVolumeClaim to request this PersistentVolume storage. Name it as *mysql-pv-claim* and request a *250Mi* of storage. Set other parameters as per your preference.
Create a deployment named *mysql-deployment*, use any mysql image as per your preference. Mount the PersistentVolume at mount path */var/lib/mysql*.
Create a *NodePort* type service named *mysql* and set nodePort to *30007*.
Create a secret named *mysql-root-pass* having a key pair value, where key is *password* and its value is *YUIidhb667*, create another secret named *mysql-user-pass* having some key pair values, where frist key is *username* and its value is *kodekloud_cap*, second key is *password* and value is *TmPcZjtRQx*, create one more secret named *mysql-db-url*, key name is *database* and value is *kodekloud_db10*
Define some Environment variables within the container:
*name: MYSQL_ROOT_PASSWORD*, should pick value from secretKeyRef *name: mysql-root-pass and key: password*
*name: MYSQL_DATABASE*, should pick value from secretKeyRef *name: mysql-db-url and key: database*
*name: MYSQL_USER*, should pick value from secretKeyRef *name: mysql-user-pass key key: username*
*name: MYSQL_PASSWORD*, should pick value from secretKeyRef *name: mysql-user-pass and key: password*

## Solution:

Create a *config.yaml* file:

```yaml
apiVersion: v1
kind: PersistentVolume
metadata:
  name: mysql-pv
spec:
  capacity:
    storage: 250Mi
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
  name: mysql-pv-claim
spec:
  accessModes:
    - ReadWriteOnce
  storageClassName: manual
  resources:
    requests:
      storage: 250Mi
---
apiVersion: apps/v1
kind: Deployment
metadata:
  name: mysql-deployment
spec:
  replicas: 1
  selector:
    matchLabels:
      app: mysql
  template:
    metadata:
      labels:
        app: mysql
    spec:
      containers:
      - name: mysql
        image: mysql:5.7
        ports:
        - containerPort: 3306
        env:
        - name: MYSQL_ROOT_PASSWORD
          valueFrom:
            secretKeyRef:
              name: mysql-root-pass
              key: password
        - name: MYSQL_DATABASE
          valueFrom:
            secretKeyRef:
              name: mysql-db-url
              key: database
        - name: MYSQL_USER
          valueFrom:
            secretKeyRef:
              name: mysql-user-pass
              key: username
        - name: MYSQL_PASSWORD
          valueFrom:
            secretKeyRef:
              name: mysql-user-pass
              key: password
        volumeMounts:
        - name: mysql-storage
          mountPath: /var/lib/mysql
      volumes:
      - name: mysql-storage
        persistentVolumeClaim:
          claimName: mysql-pv-claim
---
apiVersion: v1
kind: Service
metadata:
  name: mysql
spec:
  type: NodePort
  selector:
    app: mysql
  ports:
  - protocol: TCP
    port: 3306
    targetPort: 3306
    nodePort: 30007
---
apiVersion: v1
kind: Secret
metadata:
  name: mysql-root-pass
type: Opaque
stringData:
  password: "YUIidhb667"
---
apiVersion: v1
kind: Secret
metadata:
  name: mysql-user-pass
type: Opaque
stringData:
  username: "kodekloud_cap"
  password: "TmPcZjtRQx"
---
apiVersion: v1
kind: Secret
metadata:
  name: mysql-db-url
type: Opaque
stringData:
  database: "kodekloud_db10"
---
```

Apply the above config using

```bash
kubectl apply -f config.yaml
```

You have a Persistant Volume, Persistant Volume Claim, Pod, Service created.