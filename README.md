#  RKE v1.5.6



# 1. Update, Upgrade ubuntu, Install ansible, curl & git

```
sudo apt update && sudo apt -y full-upgrade && apt install ansible git curl -y
```

akan memakan waktu lama, setelah selesai pastikan restart server kemudian cek versi ansible dan pastikan firewall inactive


```
ansible --version
ufw status
```

# 2. Clone repo & jalankan script install rancher

```
git clone https://github.com/anakdevops/rancher-single-node.git
cd rancher-single-node
```
## pastikan username dan IP sudah sesuai didalam file install.yaml
contoh disini memakai IP 192.168.0.208 dan user serverdevops

```
ansible-playbook install.yaml
```
akan memakan waktu lama, setelah selesai pastikan restart server


# 3. pastikan kubectl, rke, helm sudah terinstall dan docker versi 24

```
kubectl version --client
rke --version
helm version
docker --version
```
## create cluster

```
sudo su
su serverdevops
cd /home/serverdevops/
rke up --config cluster.yml
```

tunggu sampai muncul { Finished building Kubernetes cluster successfully }


# 4. export KUBECONFIG dan cek nodes

```
export KUBECONFIG=$HOME/kube_config_cluster.yml
kubectl get nodes
```

# 5. Deploy UI Rancher

```
helm repo add rancher-latest https://releases.rancher.com/server-charts/latest
kubectl create namespace cattle-system
kubectl apply -f https://github.com/cert-manager/cert-manager/releases/download/v1.13.2/cert-manager.crds.yaml
helm repo add jetstack https://charts.jetstack.io
helm repo update
helm install cert-manager jetstack/cert-manager --namespace cert-manager --create-namespace --version v1.13.2
helm install rancher rancher-latest/rancher --namespace cattle-system --set hostname=rancher.tes.localorg
kubectl -n cattle-system get deploy rancher
kubectl scale --replicas=1 deployment rancher -n cattle-system
kubectl -n cattle-system get deploy rancher -w
```
