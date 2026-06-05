# Kubernetes environment for testing - K3s

## Virtual Machine
VMWare Player or any other virtualization
Install VM - for example: Ubuntu 24.04.4

1. Install docker.io:
<pre>apt install docker.io</pre>

1. Install curl:
<pre>apt install curl</pre>

1. Install K3s:
<pre>curl -sfL https://get.k3s.io | sh - </pre>

1. Permissions - to let the non-root user to execute kubectl

1. Create hidden .kube folder in your home directory
<pre>mkdir -p $HOME/.kube</pre>

1. Copy K3s config as root user to your personal folder
<pre>sudo cp /etc/rancher/k3s/k3s.yaml $HOME/.kube/config</pre>

1. Change owner of the k3s config
<pre>sudo chown $(id -u):$(id -g) $HOME/.kube/config<pre>

1. Protect the K3s configuration file
<pre>chmod 600 $HOME/.kube/config</pre>

1. Export KUBECONFIG variable
<pre>export KUBECONFIG=$HOME/.kube/config</pre>

1. Testing
<pre>kubectl get node</pre>

1. (Optional) - command autocompletion
<pre>sudo apt update
sudo apt install -y bash-completion
sudo echo 'source <(kubectl completion bash)' >> ~/.bashrc
echo 'alias k=kubectl' >> ~/.bashrc
echo 'complete -o default -F __start_kubectl k' >> ~/.bashrc
source ~/.bashrc</pre>

1. Autocompletion testing
<pre>k get namespaces
NAME              STATUS   AGE
default           Active   28m
echo              Active   23m
kube-node-lease   Active   28m
kube-public       Active   28m
kube-system       Active   28m
root@pklawit-ubuntu-arm:~#</pre>


