# kubernetes-suite

This repository contains a series of examples that can be used to run kubernetes kluster.

Folder structure:

```
└──  project            (e.g.: project-hello-world)
│    └──  source code    (e.g.: yaml)
```

Local installation and set up (Ubuntu) is explained below.

## Step 1: Install Docker

Kubernetes uses Docker as the container runtime. First, we need to install Docker.

Update the package index and install prerequisites:

```
sudo apt update
sudo apt install apt-transport-https ca-certificates curl software-properties-common
```

Add Docker’s official GPG key and repository:

```
curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo apt-key add -
sudo add-apt-repository "deb [arch=amd64] https://download.docker.com/linux/ubuntu $(lsb_release -cs) stable"
```

Install Docker:

```
sudo apt update
sudo apt install docker-ce
```

Verify Docker installation:

```
sudo systemctl status docker
```

Fixing Docker Permissions

Add your user to the docker group:

```
sudo usermod -aG docker $USER
```

Refresh group membership:

```
newgrp docker
```

Verify Docker Permissions

To check if the permissions are set correctly, run the following command:

```
docker run hello-world
```

If you see a "Hello from Docker!" message, Docker is working correctly.

## Step 2: Install Kubernetes (Minikube)

Minikube is a tool that makes it easy to run Kubernetes locally. It runs a single-node Kubernetes cluster on your machine.

Install Minikube dependencies:

```
sudo apt install -y conntrack
```

Download and install Minikube:

```
curl -LO https://storage.googleapis.com/minikube/releases/latest/minikube-linux-amd64
sudo install minikube-linux-amd64 /usr/local/bin/minikube
```

Start Minikube:

```
minikube start --driver=docker
```

Verify Minikube installation:

```
minikube status
```

## Step 3: Install kubectl, kubens, kubectx, k9s

### kubectl

kubectl is the command line tool for interacting with your Kubernetes cluster.

Download and install kubectl:

```
curl -LO "https://dl.k8s.io/release/$(curl -L -s https://dl.k8s.io/release/stable.txt)/bin/linux/amd64/kubectl"
chmod +x kubectl
sudo mv kubectl /usr/local/bin/
```

Verify kubectl installation:

```
kubectl version --client
```

### kubens and kubectx

You can install both tools by cloning their GitHub repository and copying the binaries to a directory in your PATH.

Clone the repository:

```
git clone https://github.com/ahmetb/kubectx.git ~/.kubectx
```

Copy kubectx and kubens to a directory in your PATH:

```
sudo ln -s ~/.kubectx/kubectx /usr/local/bin/kubectx
sudo ln -s ~/.kubectx/kubens /usr/local/bin/kubens
```

Verify the Installation

```
kubectx
kubens
```

### k9s

Download using Webi:

```
curl -sS https://webinstall.dev/k9s | bash
```

or check original documentation on github.

This will open the k9s interface where you can navigate and manage your Kubernetes resources.

## Step 4: Create a Kubernetes Job

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

## Step 5: Clean Up

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
