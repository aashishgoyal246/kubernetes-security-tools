# Kubernetes Kubesec

## **Table of contents**
- [Description](#description)
- [Prerequisites](#prerequisites)
- [Screenshots](#screenshots)
- [Getting Started](#getting-started)

## Description

This is a kubectl plugin for scanning Kubernetes pods, deployments, daemonsets and statefulsets with [kubesec.io](https://kubesec.io/)

## Prerequisites

1. Ubuntu System.

2. A master node and one or more worker nodes in a cluster.

## Getting Started

### Installation of krew for kubectl -

To install krew on the kubernetes cluster do the following -

```sh
curl -fsSLO "https://github.com/kubernetes-sigs/krew/releases/latest/download/krew.{tar.gz,yaml}"
tar zxvf krew.tar.gz
./krew-linux_amd64 install --manifest=krew.yaml --archive=krew.tar.gz
cat >> ~/.bashrc <<EOF
export PATH="${KREW_ROOT:-$HOME/.krew}/bin:$PATH"
EOF
source ~/.bashrc
```

### Installing kubesec-scan using krew -

Installing kubesec-scan plugin -

```sh
kubectl krew install kubesec-scan
```

### Deploying an nginx image -

Step 1. Creating a file of any name as (your filename).yml -

```sh
apiVersion: apps/v1
kind: Deployment
metadata:
  name: test
  labels:
    app: nginx
spec:
  replicas: 2
  selector:
    matchLabels:
      app: nginx
  template:
    metadata:
      labels:
        app: nginx
    spec:
      containers:
        - name: nginx
          image: nginx
          ports:
            - containerPort: 80
```

Step 2. Deploying this pod onto the cluster -

```sh
kubectl apply -f (your filename).yml
```

### Usage -

Scan a pod -

```sh
kubectl-kubesec_scan pod (pod name) -n kube-system 
```

This will result in -

```
kubesec.io score: 3
-----------------
Advise
1. containers[] .securityContext .runAsNonRoot == true
Force the running image to run as a non-root user to ensure least privilege
2. containers[] .securityContext .capabilities .drop
Reducing kernel capabilities available to a container limits its attack surface
3. containers[] .securityContext .readOnlyRootFilesystem == true
An immutable root filesystem can prevent malicious binaries being added to PATH and increase attack cost
4. containers[] .securityContext .runAsUser > 10000
Run as a high-UID user to avoid conflicts with the host's user table
5. containers[] .securityContext .capabilities .drop | index("ALL")
Drop all capabilities and add only those required to reduce syscall attack surface
```

This error is because the deployment is not secure and requires certain actions to be performed to make it secure.

### Solution -

The solution I got is that is basically for different docker image to show you that above errors are resolved as for image nginx the solution will not work because of restrictions in the nginx image.

Deploy the below yaml file which results that the deployment is secure onto the cluster.

```sh
apiVersion: apps/v1
kind: Deployment
metadata:
  name: security
  labels:
    app: security
spec:
  replicas: 2
  selector:
    matchLabels:
      app: security
  template:
    metadata:
      labels:
        app: security
    spec:
      containers:
        - name: security
          image: gcr.io/google-samples/node-hello:1.0
          ports:
            - containerPort: 80
          securityContext:
            runAsUser: 10001     
            runAsNonRoot: true
            readOnlyRootFilesystem: true
            capabilities:
              drop:
              - ALL
              add:
              - NET_BIND_SERVICE
              - NET_ADMIN
          resources:
            limits:
              cpu: 500m
              memory: 256Mi
            requests:
              cpu: 500m
              memory: 128Mi
```

Explaination -

1. (runAsUser) - This means that create a user with id more than 10000 so that it doesn't conflict with the host machine users. To create it do -

```sh
sudo useradd -u 10001 (user name)
```

2. (runAsNonRoot) - It clarifies that the container that is to being deployed should be run as a non root user. For more info - [Link](https://kubernetes.io/blog/2016/08/security-best-practices-kubernetes-deployment/)

3. (readOnlyRootFilesystem) - It denotes that if the root privileges are given than there will be a limitation on it that it can only read into the file system not write.

4. (capabilities) - It denotes that we can grant certain privileges to a process without granting all the privileges of the root user. Drop means that drop all the permissions including root ones. For more info - [Link](https://github.com/torvalds/linux/blob/master/include/uapi/linux/capability.h). In this link it gives all the capabilities that linux holds.

5. (resources) - It tells that the resources you want to limit on the container that it can use only this much resources of the host system to deploy the pod. For more info - [Link](https://kubernetes.io/docs/tasks/configure-pod-container/assign-cpu-resource/)


### Drawbacks -

If the deployment scan doesn't work that you can install yard onto your system which will create a master node and some several setups also link consul and vault. To install do the following follow the steps mentioned here - [Link](https://shipyard.demo.gs/)