//
df -h

1. Clean Docker

sudo docker system prune -a -f --volumes

or 

docker system prune -a -f
docker volume prune -f

This will remove:

unused containers
unused images
unused networks
unused volumes
build cache

2. Clean Minikube cache

minikube ssh -- docker system prune -af

3. Remove old logs

sudo journalctl --vacuum-time=3d

4. Start Minikube again

minikube start --driver=docker
minikube status

5. Reopen ArgoCD tunnel

kubectl get pods -n argocd
kubectl port-forward svc/argocd-server -n argocd 8080:443 --address 0.0.0.0

//