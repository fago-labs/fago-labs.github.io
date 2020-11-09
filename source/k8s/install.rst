Cài đặt Kubernetes
==========================================

.. contents:: Nội dung

1. Chuẩn bị
-----------------------

- Cấu hình Server: 3 VPS
- Sử dụng VPS của Google Cloud
- Cấu hình mỗi VPS: 4GB RAM 2 CPU


2. Cài đặt Kubernetes Cluster
-------------------------------

Cài đặt **Docker** bằng lệnh:
::

    curl -fsSL https://get.docker.com -o get-docker.sh
    sh get-docker.sh


SSH đến các VPS và chạy lệnh cài đặt K8s:
::

    sudo apt-get update && sudo apt-get install -y apt-transport-https curl
    curl -s https://packages.cloud.google.com/apt/doc/apt-key.gpg | sudo apt-key add -
    cat <<EOF | sudo tee /etc/apt/sources.list.d/kubernetes.list
    deb https://apt.kubernetes.io/ kubernetes-xenial main
    EOF
    sudo apt-get update
    sudo apt-get install -y kubelet kubeadm kubectl
    sudo apt-mark hold kubelet kubeadm kubectl


Master Node
~~~~~~~~~~~~~~~~~~~~~~~~~

Cài đặt Kubernetes Cluster với network plugin Calico

SSH đến VPS Master và chạy lệnh sau để khởi tạo node Master
::

    kubeadm init --pod-network-cidr=192.168.0.0/16

Đợi quá trình khởi tạo hoàn tất, chạy lệnh sau:
::

    mkdir -p $HOME/.kube
    sudo cp -i /etc/kubernetes/admin.conf $HOME/.kube/config
    sudo chown $(id -u):$(id -g) $HOME/.kube/config

Cài đặt plugin Calico:
::

    kubectl create -f https://docs.projectcalico.org/manifests/tigera-operator.yaml
    kubectl create -f https://docs.projectcalico.org/manifests/custom-resources.yaml
    kubectl taint nodes --all node-role.kubernetes.io/master-

Chạy lệnh sau để lấy join-command
::

    kubeadm token create --print-join-command

Master node sẽ trả về 1 join command dùng để chạy trên Worker để tham gia vào cluster
::

    root@master:~# kubeadm token create --print-join-command
    kubeadm join 10.148.0.6:6443 --token o7gqha.c89gpgn8kt3jgckt     --discovery-token-ca-cert-hash sha256:6401eb108c8d75e558dd2d6207b1d88b1eca8602d8e7eea71f08da2643a27f19 
    root@master:~#

Worker Node
~~~~~~~~~~~~~~~~~~~~~~~~~

SSH đến Worker Node và nhập lệnh join command từ Master Node trước đó

Đợi tầm vài phút, chạy lệnh sau trên Master Node để kiểm tra các Worker Node đã tham gia cluster chưa:
::

    root@master:~# kubectl get nodes
    NAME      STATUS   ROLES    AGE    VERSION
    master    Ready    master   6d6h   v1.19.2
    worker1   Ready    <none>   6d5h   v1.19.2
    worker2   Ready    <none>   6d5h   v1.19.2
    root@master:~#

Quá trình cài đặt Kubernetes hoàn tất











































