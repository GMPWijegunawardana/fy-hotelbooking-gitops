#!/bin/bash

# ================================
# 🚀 EC2 Minikube + ArgoCD Startup Script
# ================================

# 1️⃣ Check system resources
echo "🔹 Checking system resources..."
df -h
free -h
nproc
echo ""

# 2️⃣ Start Docker service
echo "🔹 Starting Docker service..."
sudo systemctl start docker
sudo systemctl status docker --no-pager
echo ""

# 3️⃣ Optional: Clean Docker if needed
read -p "Do you want to clean Docker unused images/containers? (y/N): " CLEAN_DOCKER
if [[ "$CLEAN_DOCKER" == "y" || "$CLEAN_DOCKER" == "Y" ]]; then
    echo "🔹 Cleaning Docker..."
    sudo docker system prune -a -f --volumes
fi
echo ""

# 4️⃣ Start Minikube
echo "🔹 Starting Minikube..."
minikube start \
  --driver=docker \
  --cpus=2 \
  --memory=3500 \
  --disk-size=20g
echo ""

# 5️⃣ Verify Minikube
echo "🔹 Verifying Minikube status..."
minikube status
kubectl get nodes
echo ""

# 6️⃣ Check ArgoCD namespace & pods
echo "🔹 Checking ArgoCD namespace and pods..."
kubectl get namespaces
kubectl get pods -n argocd
echo ""

# 7️⃣ Get ArgoCD initial admin password
echo "🔹 Retrieving ArgoCD admin password..."
ARGOCD_PASSWORD=$(kubectl get secret argocd-initial-admin-secret -n argocd -o jsonpath="{.data.password}" | base64 --decode)
echo "✅ ArgoCD Password: $ARGOCD_PASSWORD"
echo ""

# 8️⃣ Start ArgoCD port-forward to access UI
echo "🔹 Starting ArgoCD port-forward..."
echo "Use Ctrl+C to stop port-forward when needed."
echo "Access ArgoCD UI at: https://<EC2_PUBLIC_IP>:8080"
kubectl port-forward svc/argocd-server -n argocd 8080:443 --address 0.0.0.0
