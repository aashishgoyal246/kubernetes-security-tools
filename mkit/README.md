# Kubernetes Inspection tool -

## **Table of contents**
- [Description](#description)
- [Prerequisites](#prerequisites)
- [Screenshots](#screenshots)
- [Getting Started](#getting-started)

## Description

MKIT provides security-minded Kubernetes cluster administrators with a quick way to assess several common misconfigurations in their Kubernetes environment.

The base github repository used - [Github](https://github.com/darkbitio/mkit)

## Prerequisites

Ubuntu System.

## Feature

* Supported Platform - Kubelet

* Not Supported Platform - Shipyard

## Screenshots 

![output](https://github.com/aashishgoyal246/kubernetes-security-tools/blob/master/mkit/mkit-k8s-security.png)

## Getting Started

### Setting up the cluster -

Step 1. To setup a cluster, here use we will setup kubernetes cluster using this script which is in same repository -

Script - [master.sh](https://github.com/clouddrove/research-and-development/blob/slave/k8s-security-tools/mkit/master.sh)

Do the following steps -

```sh
sudo sh (your filename).sh
mkdir .kube
sudo cp -i /etc/kubernetes/admin.conf .kube/config
sudo chown -R $(id -u):$(id -g) .kube
kubectl config set-context --current --namespace=kube-system
kubectl apply -f https://raw.githubusercontent.com/coreos/flannel/2140ac876ef134e0ed5af15c65e414cf26827915/Documentation/kube-flannel.yml
kubectl taint nodes --all node-role.kubernetes.io/master-
```

Step 2. Now verify that cluster has been created or not -

```sh
kubectl get nodes
```

### Usage

Step 1. To setup inspection tool onto your cluster do the following -

```sh
git clone https://github.com/darkbitio/mkit
```

```sh
cd mkit
make run-k8s
```

Step 2. Now to access the dashboard of mkit do the following -

```sh
http://localhost:8000
```

### Result

Doing the above steps you will be able to check the risk severity onto your cluster by doing inspection on the cluster.