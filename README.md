
# 1. Update, Upgrade ubuntu, Install ansible & git

```
sudo apt update && sudo apt -y full-upgrade && apt install ansible git -y
```

akan memakan waktu lama, setelah selesai pastikan restart server


# 2. Pengecekan versi ansible dan pastikan firewall inactive

```
ansible --version
ufw status
```

# 3. Clone repo & jalankan script install rancher

```
git clone https://github.com/anakdevops/rancher-single-node.git
cd rancher-single-node
```
## pastikan username dan IP sudah sesuai didalam file install.yaml
contoh disini memakai IP 192.168.0.208 dan user anakdevops

```
ansible-playbook install.yaml
```
akan memakan waktu lama, setelah selesai pastikan restart server


# 4. pastikan kubectl, rke, helm sudah terinstall

```
kubectl --version
rke --version
helm --version
```
