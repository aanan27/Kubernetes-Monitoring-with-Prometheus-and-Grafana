Sure, here's an expanded version of the README with more detailed steps:

---

# Kubernetes Monitoring with Prometheus and Grafana

This project aims to set up monitoring for a Kubernetes cluster using Prometheus and Grafana.

## Prerequisites

Before getting started, ensure you have the following installed:

- [Minikube](https://minikube.sigs.k8s.io/docs/start/)
- [Helm](https://helm.sh/docs/intro/install/)
- [kubectl](https://kubernetes.io/docs/tasks/tools/install-kubectl/)

## Installation

### Step 1: Start Minikube

Start Minikube to create a local Kubernetes cluster.

```bash
minikube start
```

### Step 2: Install Prometheus

Add the Prometheus Helm repository and install Prometheus using Helm.

```bash

helm repo add prometheus-community https://prometheus-community.github.io/helm-charts
helm repo update

helm install prometheus prometheus-community/prometheus


kubectl expose service prometheus-server --type=NodePort --target-port=9090 --name=prometheus-server-ext

minikube service prometheus-server-ext
```

### Step 3: Install Grafana

Add the Grafana Helm repository and install Grafana using Helm.

```bash

helm repo add grafana https://grafana.github.io/helm-charts
helm repo update


helm install grafana stable/grafana


kubectl expose service grafana --type=NodePort --target-port=3000 --name=grafana-ext

minikube service grafana-ext
```

### Step 4: Retrieve Grafana Credentials

Retrieve the username and password for Grafana.

```bash
kubectl get secret --namespace default grafana -o jsonpath="{.data.admin-password}" | base64 --decode ; echo
```

or

kubectl get secret --namespace default grafana -o yaml | grep "password:" | awk '{print $2}' | base64 --decode ; echo
```

## Accessing Grafana

To access Grafana, port-forward the Grafana pod to your local machine.

```bash
kubectl --namespace default port-forward <grafana-pod-name> 3000:3000
```

Then, open your web browser and navigate to [http://localhost:3000](http://localhost:3000). Log in with the provided credentials.

## Dashboard Setup

Import Grafana dashboards for Kubernetes and Prometheus metrics. Use the following resources:

- [Grafana Dashboards](https://grafana.com/grafana/dashboards)
- [Prometheus Dashboards](https://prometheus.io/docs/visualization/grafana/)


