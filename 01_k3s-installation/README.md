# Kubernetes environment for testing - K3s
K3s is a single-VM Kubernetes instance
More info about the project: https://k3s-io.github.io/

## Virtual Machine - create
Use VMWare Player or any other virtualization software and create Linux VM there.
In below examples all the commands will be compatible with Ubuntu 24.04.4
Network: create at least Host network, so 

## Virtual Machine - install K3s and necessary tools
1. Install docker.io:
'''bash
sudo apt install docker.io

2. Install curl:
<pre>sudo apt install curl</pre>

3. Install K3s:
<pre>sudo curl -sfL https://get.k3s.io | sh - </pre>

4. Test as root user:
<pre>kubectl get node</pre>

## Virtual Machine - prepare config for non-root user
1. Create .kube folder in your home directory
<pre>mkdir -p $HOME/.kube</pre>

2. Copy K3s config as root user to your personal folder
<pre>sudo cp /etc/rancher/k3s/k3s.yaml $HOME/.kube/config</pre>

3. Change owner of the k3s config:
<pre>sudo chown $(id -u):$(id -g) $HOME/.kube/config</pre>

4. Protect the K3s configuration file:
<pre>chmod 600 $HOME/.kube/config</pre>

5. Export KUBECONFIG variable:
<pre>export KUBECONFIG=$HOME/.kube/config</pre>

6. Testing
<pre>kubectl get node</pre>

7. (Optional) - command autocompletion
<pre>sudo apt update
sudo apt install -y bash-completion
sudo echo 'source <(kubectl completion bash)' >> ~/.bashrc
echo 'alias k=kubectl' >> ~/.bashrc
echo 'complete -o default -F __start_kubectl k' >> ~/.bashrc
source ~/.bashrc</pre>

8. (Optional) Autocompletion testing
<pre>k get namespaces
NAME              STATUS   AGE
default           Active   28m
echo              Active   23m
kube-node-lease   Active   28m
kube-public       Active   28m
kube-system       Active   28m
</pre>

## (Optional) Connecting to the cluster from another machine
This will be the typical use-case, that Kubernetes cluster is installed in a cloud environment, and you will be deploying applications/PODs to it from remote machine.

1. Check the node IP address:
<pre>kubectl get node -o wide
NAME                 STATUS   ROLES           AGE    VERSION        INTERNAL-IP    EXTERNAL-IP   OS-IMAGE             KERNEL-VERSION      CONTAINER-RUNTIME
pklawit-ubuntu-arm   Ready    control-plane   5d8h   v1.35.5+k3s1   192.168.64.6   <none>        Ubuntu 24.04.4 LTS   6.17.0-35-generic   containerd://2.2.3-k3s1
</pre>
2. Check if K3s VM listens on port 6443
ss -tulpan | grep 6443
tcp   LISTEN    0      4096                       *:6443                      *:*

3. Copy .kube/config to your local machine, update the IP address in config to the one from point 1.

4. Export KUBECONFIG variable

5. Test 'kubectl' command


