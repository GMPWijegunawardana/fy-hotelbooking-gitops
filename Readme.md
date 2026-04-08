#!/bin/bash

# ----------------------------------------
# Script to start Minikube & ArgoCD on EC2
# ----------------------------------------

echo "1️⃣ Checking system resources..."
df -h
free -h
nproc

echo "2️⃣ Starting Docker service..."
sudo systemctl start docker

# Optional: Clean Docker if needed (run manually only if out of disk space)
# echo "Cleaning Docker..."
# sudo docker system prune -a -f --volumes

echo "3️⃣ Starting Minikube..."
minikube start \
  --driver=docker \
  --cpus=2 \
  --memory=3500 \
  --disk-size=20g

echo "4️⃣ Verifying Minikube..."
minikube status
kubectl get nodes

echo "5️⃣ Checking ArgoCD namespace & pods..."
kubectl get namespaces
kubectl get pods -n argocd

echo "6️⃣ Starting ArgoCD port-forward..."
echo "Use CTRL+C to stop port-forward when done"
kubectl port-forward svc/argocd-server -n argocd 8080:443 --address 0.0.0.0
