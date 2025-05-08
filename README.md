# 🚀 MinIO Deployment Using Helm Charts

This guide walks you through deploying [MinIO](https://min.io/) on a Kubernetes cluster using Helm charts. You'll clone the chart from GitHub, install MinIO, check service readiness, and optionally install the NGINX Ingress controller for Ingress-based routing.

---

## 📦 Prerequisites

Make sure you have the following installed and configured:

- Kubernetes cluster (local or cloud)
- `kubectl` configured to communicate with the cluster
- Helm 3.x installed
- docker installed
- Internet access
- open port 30000,310000
- add "<public-ip> minio-api.local minio-console.local" this line in /etc/hosts to access on localhost

---

## 🔁 Clone the Helm Chart

First, clone the Helm chart repository:

```bash
git clone https://github.com/amolsontakke96/minio
cd minio

📥 Deploy MinIO Using Helm

Install the MinIO Helm chart from the local minio/ directory:

```bash
helm install minio minio/

✅ Verify the Deployment

Check Pods

```bash
kubectl get pods

Check Services

```bash
kubectl get svc

✅ Install NGINX Ingress Controller via Helm
1. Add the Ingress NGINX Helm repository

```bash
helm repo add ingress-nginx https://kubernetes.github.io/ingress-nginx
helm repo update

2. Install the Ingress Controller

```bash
Use the following command to install it in the ingress-nginx namespace:

helm install ingress-nginx ingress-nginx/ingress-nginx \
  --namespace ingress-nginx \
  --create-namespace

3. Verify the Installation

Check that the controller pod is running:
```bash
kubectl get pods -n ingress-nginxkubectl get pods -n ingress-nginx

4. Change the nginx controller type from load balancer to Nodeport

```bash
helm install ingress-nginx ingress-nginx/ingress-nginx \
  --namespace ingress-nginx \
  --create-namespace \
  --set controller.service.type=NodePort \
  --set controller.service.nodePorts.http=30080 \
  --set controller.service.nodePorts.https=30443

🌐 Access MinIO Services
API Health Check
```bash

curl -I http://<public-ip>:30000/minio/health/ready

Web UI Access

curl -I http://<public-ip>:31000

Or open in your browser:

http://<public-ip>:31000







