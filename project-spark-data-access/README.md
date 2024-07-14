# Simple Python script with Spark Job

## Step 1: Prepare the Python Script

First, you need a Python script that prints "Hello, World!". Let's call this script hello.py.

```
print("Hello, World!")
```

### Step 2: Create a Dockerfile

To run the Python script in the ubuntu:latest image, you need to create a Dockerfile to set up the environment and add the script to the image.

Dockerfile

```
# Use the latest Ubuntu image
FROM ubuntu:latest

# Install Python
RUN apt-get update && \
    apt-get install -y python3

# Copy the Python script into the container
COPY hello.py /hello.py

# Set the default command to execute the Python script
CMD ["python3", "/hello.py"]
```

## Step 3: Build and Push the Docker Image

Build the Docker image:

```
docker build -t my-python-job:latest .
```

See the docker image just built by running:

```
docker images
```

Test the docker image just build by running:

```
docker run my-python-job:latest
```

Once tested successfully, push the Docker image to a container registry (e.g., Docker Hub). First, tag the image:

```
docker tag my-python-job:latest your_dockerhub_username/my-python-job:latest
```

Then, push it to Docker Hub:

```
docker push your_dockerhub_username/my-python-job:latest
```

Replace your_dockerhub_username with your actual Docker Hub username.

## Step 4: Create the Kubernetes Job YAML

Now, create a YAML file for the Kubernetes Job that uses this Docker image.

```
apiVersion: batch/v1
kind: Job
metadata:
name: python-hello-world
spec:
template:
spec:
containers: - name: python-container
image: your_dockerhub_username/my-python-job:latest
imagePullPolicy: Always
restartPolicy: Never
backoffLimit: 4
```

Replace your_dockerhub_username/my-python-job:latest with the image you pushed to Docker Hub.

## Step 5: Apply the Kubernetes Job

Apply the YAML configuration to create the Job:

```
kubectl apply -f python-job.yaml
```

## Step 6: Verify the Job

Check the status of the Job and pods:

```
kubectl get jobs
kubectl get pods
```

## Step 7: Check Logs

To see the output of the Python script, view the logs of the pod created by the Job:

```
kubectl logs <pod-name>
```

Replace <pod-name> with the actual name of the pod created by the Job.
