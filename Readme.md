
1. first need to check k3s is running?
sudo systemctl status k3s
if not running 
sudo systemctl start k3s

2. fix cubectl (if needed)
export KUBECONFIG=/etc/rancher/k3s/k3s.yaml
or
mkdir -p ~/.kube
sudo cp /etc/rancher/k3s/k3s.yaml ~/.kube/config
sudo chown $USER:$USER ~/.kube/config

3. access the url ( https ,server ip , nodeport)
https://IP:31430
