# 5G Core Infrastructure Setup (Open5GS)

This document outlines the complete step-by-step guide to prepare an Ubuntu Linux environment and deploy the Open5GS 5G Core network using Minikube and Helm. It includes a detailed engineering breakdown of every command used.

## 1. System Preparation & Docker Installation
Docker is the containerization engine that will host our virtual Kubernetes nodes.

```bash
sudo apt update
sudo apt install curl -y
sudo apt install -y docker.io
sudo usermod -aG docker $USER
```

**🔍 Engineering Breakdown:**
* `apt update`: Requests the Ubuntu system to update the list of available packages from the official repositories to ensure the latest versions are downloaded.
* `install curl -y`: Installs the `curl` utility, which is responsible for pulling files from web URLs. The `-y` flag automatically answers 'yes' to space allocation prompts.
* `install -y docker.io`: Installs the Docker container engine.
* `usermod -aG docker $USER`: Appends (`-aG`) the current user (`$USER`) to the `docker` group. This grants the user permissions to control Docker without needing `sudo` for every container execution, which is a strict prerequisite for running Minikube.

> **CRITICAL NOTE:** After running the above commands, you MUST run `su - $USER` (and enter your password) to apply Docker permissions in your current terminal session, otherwise Minikube will throw a `permission denied` error.

---

## 2. Minikube Installation
Minikube is a lightweight Kubernetes implementation that creates a VM or container on our local machine and deploys a simple cluster containing only one node.

```bash
curl -LO [https://storage.googleapis.com/minikube/releases/latest/minikube-linux-amd64](https://storage.googleapis.com/minikube/releases/latest/minikube-linux-amd64)
sudo install minikube-linux-amd64 /usr/local/bin/minikube
rm minikube-linux-amd64
```

**🔍 Engineering Breakdown:**
* `curl -LO`: Downloads the Minikube executable. The `L` flag follows HTTP redirects, and the `O` flag saves the file using its original remote name.
* `sudo install ... /usr/local/bin/minikube`: Moves the downloaded file to the system's primary executable path (`/usr/local/bin`), renames it to `minikube`, and grants it execution permissions.
* `rm`: Removes the original downloaded installation file to free up storage space.

---

## 3. Kubectl Installation
`kubectl` is the Kubernetes command-line tool. It allows us to run commands against Kubernetes clusters.

```bash
curl -LO "[https://dl.k8s.io/release/$(curl](https://dl.k8s.io/release/$(curl) -L -s [https://dl.k8s.io/release/stable.txt)/bin/linux/amd64/kubectl](https://dl.k8s.io/release/stable.txt)/bin/linux/amd64/kubectl)"
sudo install -o root -g root -m 0755 kubectl /usr/local/bin/kubectl
rm kubectl
```

**🔍 Engineering Breakdown:**
* `$(curl -L -s .../stable.txt)`: A nested command. It first silently (`-s`) fetches the latest stable K8s version number, then embeds it into the primary URL to download the exact compatible version of `kubectl`.
* `install -o root -g root -m 0755`: Installs the tool, sets the owner and group to `root`, and assigns `0755` permissions (owner can modify; everyone can execute).

---

## 4. Helm Installation
Helm is the package manager for Kubernetes (similar to `apt` for Ubuntu). We use it to deploy the complex 5G Core microservices easily.

```bash
sudo snap install helm --classic
```

**🔍 Engineering Breakdown:**
* `snap`: Another package manager in Ubuntu that installs applications with all their dependencies in isolated environments.
* `--classic`: Grants the Helm snap broader system access permissions, which is necessary for infrastructure management tools.

---

## 5. Start the Minikube Cluster

```bash
minikube start --driver=docker
```

**🔍 Engineering Breakdown:**
* `--driver=docker`: Instructs Minikube not to spin up a heavy virtual machine (VM). Instead, it builds the cluster as a lightweight container inside Docker to optimize system resource consumption.

---

## 6. Deploy Open5GS (OCI Registry & Kernel Fix)
Deploying the actual 5G Core Network architecture.

```bash
helm install my-open5gs oci://registry-1.docker.io/gradiantcharts/open5gs --version 2.4.1 --set mongodb.image.tag=7.0.12
```

**🔍 Engineering Breakdown:**
* `helm install my-open5gs`: The command to build and install the network, assigning it the release name `my-open5gs`.
* `oci://.../open5gs`: Instead of using standard web repositories that might fail (e.g., 404 Errors), we use the OCI protocol to pull the official Gradiant chart directly from Docker Hub servers.
* `--version 2.4.1`: Locks the installation to a specific, stable version of the chart to prevent future updates from breaking the environment.
* `--set mongodb.image.tag=7.0.12`: **(Crucial Fix)** The default chart relies on an older MongoDB version that crashes (`CrashLoopBackOff`) on Linux systems running kernels newer than `6.1.9`. This parameter forces Helm to replace the old version with `7.0.12`, which is fully compatible with modern kernels, ensuring network stability.

---

## 7. Verify Deployment

```bash
kubectl get pods
```

**🔍 Engineering Breakdown:**
* `get pods`: Queries the status of all Pods (the smallest execution units in Kubernetes hosting network functions like AMF and UPF). We monitor this until all components transition to the `Running` state.