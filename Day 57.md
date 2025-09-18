## Day 57: Print Environment Variables

The DevOps team is working on to setup some pre-requisites for an application that will send the greetings to different users. There is a sample deployment, that needs to be tested. Below is a scenario which needs to be configured on Kubernetes cluster. Please find below more details about it.


Create a *pod* named *print-envars-greeting*.

Configure spec as, the container name should be *print-env-container* and use *bash* image.

Create three environment variables:

a. *GREETING* and its value should be *Welcome to*
b. *COMPANY* and its value should be *Stratos*
c. *GROUP* and its value should be *Industries*

Use command *["/bin/sh", "-c", 'echo "$(GREETING) $(COMPANY) $(GROUP)"']* (please use this exact command), also set its *restartPolicy* policy to *Never* to avoid crash loop back.

You can check the output using *kubectl logs -f print-envars-greeting* command.

## Solution:

Create the below "env-var.yaml" file,

```yaml
apiVersion: v1
kind: Pod
metadata:
  name: print-envars-greeting
spec:
  containers:
  - name: print-env-container
    image: bash
    env:
    - name: GREETING
      value: Welcome to
    - name: COMPANY
      value: Stratos
    - name: GROUP
      value: Industries
    command: ["/bin/sh", "-c", 'echo "$(GREETING) $(COMPANY) $(GROUP)"']
    restartPolicy: Never
---
```

Apply the configuration using the command,

```bash
kubectl apply -f env-var.yaml
```

This will run the bash image and prints the environment variables.

To verify the greeting message,

```bash
kubectl logs -f print-envars-greeting  #This will print the log ""Welcome to Stratos Industries"".
```
