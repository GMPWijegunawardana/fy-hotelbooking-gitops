# Check system resources
df -h
free -h
nproc

# Start Docker service
sudo systemctl start docker

# (Optional) Clean Docker if needed
# Only run if you run out of disk space
# sudo docker system prune -a -f --volumes

# Start Minikube
minikube start \
  --driver=docker \
  --cpus=2 \
  --memory=3500 \
  --disk-size=20g

# Verify Minikube
minikube status
kubectl get nodes

# Check ArgoCD namespace & pods
kubectl get namespaces
kubectl get pods -n argocd

# Start ArgoCD port-forward to access UI
kubectl port-forward svc/argocd-server -n argocd 8080:443 --address 0.0.0.0
