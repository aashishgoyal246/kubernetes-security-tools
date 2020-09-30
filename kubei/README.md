# Kubernetes Vulnerability Inspection tool -

## **Table of contents**
- [Description](#description)
- [Prerequisites](#prerequisites)
- [Screenshots](#screenshots)
- [Getting Started](#getting-started)

## Description

Kubei is a flexible Kubernetes runtime scanner, scanning images of worker and Kubernetes nodes providing accurate vulnerabilities assessment.

Kubei is a vulnerabilities scanning tool that allows users to get an accurate and immediate risk assessment of their kubernetes clusters. Kubei scans all images that are being used in a Kubernetes cluster, including images of application pods and system pods.

The base github repository used - [Github](https://github.com/Portshift/kubei)

## Prerequisites

Ubuntu System.

## Feature

* Supported Platform - Kubelet

* Not Supported Platform - Shipyard

## Screenshots 

![output](https://github.com/aashishgoyal246/kubernetes-security-tools/blob/master/kubei/kubei-k8s-security.png)

## Getting Started

### Setting up the cluster -

Step 1. To setup a cluster, here use we will setup kubernetes cluster using this script which is in same repository -

Script - [master.sh](https://github.com/clouddrove/research-and-development/blob/slave/k8s-security-tools/kubei/master.sh)

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
kubectl apply -f https://raw.githubusercontent.com/Portshift/kubei/master/deploy/kubei.yaml
kubectl -n kubei get pod -lapp=kubei
kubectl -n kubei port-forward $(kubectl -n kubei get pods -lapp=kubei -o jsonpath='{.items[0].metadata.name}') 8080
```

Step 2. Now to access the dashboard of kubei do the following and click on `GO` to run the scan -

```sh
http://localhost:8080/view/
```

### Result

Doing the above steps you will be able to check the risk severity onto your cluster by doing inspection of all the images on the cluster.