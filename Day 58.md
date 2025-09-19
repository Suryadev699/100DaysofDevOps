## Day 58: Deploy Grafana on Kubernetes Cluster

The DevOps teams is planning to set up a Grafana tool to collect and analyze analytics from some applications. They are planning to deploy it on Kubernetes cluster. Below you can find more details.

Create a deployment named *grafana-deployment-xfusion* using any *grafana* image for Grafana app. Set other parameters as per your choice.

Create *NodePort* type service with *nodePort* *32000* to expose the app.

You need not to make any configuration changes inside the Grafana app once deployed, just make sure you are able to access the Grafana login page.

## Solution:

Create a *grafana-deployment.yaml* file,

```yaml
---
apiVersion: apps/v1
kind: Deployment
metadata:
  name: grafana-deployment-xfusion
  labels:
    app: grafana
spec:
  replicas: 1
  selector:
    matchLabels:
      app: grafana
  template:
    metadata:
      labels:
        app: grafana
    spec:
      containers:
      - name: grafana-container
        image: grafana:latest
        imagePullPolicy: IfNotPresent
        ports:
        - containerPort: 3000
---
apiVersion: v1
kind: Service
metadata:
  name: grafana-service
  labels:
    app: grafana
spec:
  type: NodePort
  selector:
    app: grafana
  ports:
  - port: 3000
    targetPort: 3000
    nodePort: 32000
---
```

Apply this configuration using,

```bash
kubectl apply -f grafana-deployment.yaml
```

You now have grafana deployed.
