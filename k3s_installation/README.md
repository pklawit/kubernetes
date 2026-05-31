Install VM - for example: Ubuntu 24.04.4

Install docker.io
apt install docker.io

Install curl:
apt install curl

Install K3s:
curl -sfL https://get.k3s.io | sh -

Permissions - to let the non-root user to execute kubectl

# 1. Tworzymy ukryty katalog .kube w Twoim folderze domowym
mkdir -p $HOME/.kube

# 2. Kopiujemy konfigurację K3s jako root do Twojego katalogu
sudo cp /etc/rancher/k3s/k3s.yaml $HOME/.kube/config

# 3. Zmieniamy właściciela pliku na Twojego użytkownika
sudo chown $(id -u):$(id -g) $HOME/.kube/config

# 4. (Opcjonalnie) Zabezpieczamy plik, dając prawa odczytu tylko Tobie
chmod 600 $HOME/.kube/config

chmod a+rw /etc/rancher/k3s/k3s.yaml

Autokompletacja
sudo apt update
sudo apt install -y bash-completion

root@pklawit-ubuntu-arm:~# echo 'source <(kubectl completion bash)' >> ~/.bashrc
root@pklawit-ubuntu-arm:~# echo 'alias k=kubectl' >> ~/.bashrc
echo 'complete -o default -F __start_kubectl k' >> ~/.bashrc
root@pklawit-ubuntu-arm:~# source ~/.bashrc

root@pklawit-ubuntu-arm:~# k get namespaces 
NAME              STATUS   AGE
default           Active   28m
echo              Active   23m
kube-node-lease   Active   28m
kube-public       Active   28m
kube-system       Active   28m
root@pklawit-ubuntu-arm:~# 


