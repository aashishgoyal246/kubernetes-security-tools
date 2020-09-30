# Kubernetes Kube-bench -

## **Table of contents**
- [Description](#description)
- [Prerequisites](#prerequisites)
- [Screenshots](#screenshots)
- [Getting Started](#getting-started)

## Description

kube-bench is a Go application that checks whether Kubernetes is deployed securely by running the checks documented in the [CIS Kubernetes Benchmark](https://www.cisecurity.org/benchmark/kubernetes/).

Tests are configured with YAML files, making this tool easy to update as test specifications evolve.

The base github repository used - [Github](https://github.com/aquasecurity/kube-bench)

## Prerequisites

Ubuntu System.

## Feature

* Supported Platform - Kubelet

* Not Supported Platform - Shipyard

## Screenshots 

![output](https://raw.githubusercontent.com/aquasecurity/kube-bench/master/images/output.png)

## Getting Started

### Setting up the cluster -

Step 1. To setup a cluster, here use we will setup kubernetes cluster using this script which is in same repository -

Script - [master.sh](https://github.com/clouddrove/research-and-development/blob/slave/k8s-security-tools/kube-bench/master.sh)

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
git clone https://github.com/aquasecurity/kube-bench
```

```sh
cd kube-bench
kubectl apply -f job.yaml
```

Step 2. Now to get the logs of the pod do the following -

```sh
kubectl get pods
```

Get the pod of the kube-bench and check logs by -

```sh
kubectl logs (pod name)
```

If you want to store the logs do the following -

```sh
kubectl logs (pod name) > logs.txt
```

### Result

Doing the above steps you will be able to get the logs of your cluster and able to check that what steps should be taken to reduce the risk on the cluster. 