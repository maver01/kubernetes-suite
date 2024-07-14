# Create Hello World K8 job

## Step 1: Create YAML config file

A Kubernetes Job runs one or more pods to completion.

Create a hello-world-job.yaml file with the following content:

```
apiVersion: batch/v1
kind: Job
metadata:
  name: hello-world
spec:
  template:
    spec:
      containers:
      - name: hello
        image: busybox
        command: ["echo", "Hello, World!"]
      restartPolicy: Never
  backoffLimit: 4
```

Set docker context:

```
docker context use default
```

Apply the Job configuration to your cluster:

```
kubectl apply -f hello-world-job.yaml
```

Verify the Job is created and check its status:

```
kubectl get jobs
```

Check the logs of the Job to see the output:

```
kubectl logs job/hello-world
```

## Step 2: Clean Up

After verifying that your Job has run successfully, you might want to clean up the resources.

Delete the Job:

```
kubectl delete -f hello-world-job.yaml
```

Stop Minikube (optional):

```
minikube stop
```

Delete Minikube cluster (optional):

```
minikube delete
```
