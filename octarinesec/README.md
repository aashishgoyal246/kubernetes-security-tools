# Kubernetes Kubescan using Octarinesec -

## **Table of contents**
- [Description](#description)
- [Prerequisites](#prerequisites)
- [Screenshots](#screenshots)
- [Getting Started](#getting-started)

## Description

Kube-Scan gives a risk score, from 0(no risk) to 10(high risk) for each workload. The risk is based on the runtime configuration of each workload. The exact rules and scoring formula are part of the open source framework KCCSS - Kubernetes Common Configuration Scoring System.

The base github repository used - [Github](https://github.com/octarinesec/kube-scan)

## Prerequisites

Ubuntu System.

## Feature

* Supported Platform - Shipyard

* Not Supported Platform - Kubelet

## Screenshots 

![output-1](https://github.com/aashishgoyal246/kubernetes-security-tools/blob/master/octarinesec/octarinesec-dashboard.png)

![output-2](https://github.com/aashishgoyal246/kubernetes-security-tools/blob/master/octarinesec/octarinesec-resource.png)

## Getting Started

### Setting up the cluster -

Step 1. To setup a cluster, here use shipyard which will create a master node and several hashicorp tools like vault and consul. To setup do the following - 

```sh
curl -sL https://shipyard.demo.gs/install.sh | bash
yard up
export KUBECONFIG="$HOME/.shipyard/yards/shipyard/kubeconfig.yml"
```

Step 2. Now verify that cluster has been created or not -

```sh
kubectl get nodes
```

### Usage

Step 1. To setup kube-scan onto your cluster do the following -

```sh
kubectl apply -f https://raw.githubusercontent.com/octarinesec/kube-scan/master/kube-scan.yaml
kubectl port-forward --namespace kube-scan svc/kube-scan-ui 8080:80
```

Step 2. Now to access the dashboard of kube-scan do -

```sh
http://localhost:8080
```

### Result

Doing the above steps you will be able to check the risk severity onto your cluster.