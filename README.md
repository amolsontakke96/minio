# 🚀 MinIO Deployment Using Helm Charts

This guide walks you through deploying [MinIO](https://min.io/) on a Kubernetes cluster using Helm charts. You'll clone the chart from GitHub, install MinIO, check service readiness, and optionally install the NGINX Ingress controller for Ingress-based routing.

---

## 📦 Prerequisites

Make sure you have the following installed and configured:

- Kubernetes cluster (local or cloud)
- `kubectl` configured to communicate with the cluster
- Helm 3.x installed
- docker installed
- For TLS Purpose 
1 Run the following command to create key and certificate

```bash
openssl req -x509 -nodes -days 365 -newkey rsa:2048 \
  -keyout tls.key -out tls.crt \
  -subj "/CN=minio-console.local/O=minio-console.local"

2 Run the following command to create secret

``` bash
kubectl create secret tls minio-tls \
  --cert=tls.crt \
  --key=tls.key \
  --namespace=default

---

## 🔁 Clone the Helm Chart

First, clone the Helm chart repository:

```bash
git clone https://github.com/amolsontakke96/minio
cd minio

📥 Deploy MinIO Using Helm

Install the MinIO Helm chart from the local minio/ directory:

```bash
helm install minio .

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
  --create-namespace \
  --set controller.hostNetwork=true

3. Verify the Installation

Check that the controller pod is running:
```bash
kubectl get pods -n ingress-nginx

4 Add the following line
add <vm-public-ip> minio-console.local this line in /etc/hosts to access on localhost

3 Run command to check if TLS is working

```bash
curl -k https://minio-console.local

🌐 Access MinIO Services

Web UI Access

curl -I http://minio-console.local

USERNAME AND PASSWORD TO ACCESS WEBUI

Username: minioadmin
Password: minioadmin



