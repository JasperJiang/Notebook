# Install in Ubuntu

### 参考网站

[https://kubernetes.io/docs/setup/independent/create-cluster-kubeadm/ ](https://kubernetes.io/docs/setup/independent/create-cluster-kubeadm/%20)

### 安装相应软件

```bash
apt-get update 
apt-get install -y curl apt-transport-https
curl -s https://packages.cloud.google.com/apt/doc/apt-key.gpg | apt-key add -

cat <<EOF >/etc/apt/sources.list.d/kubernetes.list
deb http://apt.kubernetes.io/ kubernetes-xenial main
EOF

apt-get update
#如果没有安装docker的需要先安装docker，我用官网最新版本的docker遇到版本不匹配的情况，所以还是按照官方教程做比较好
apt-get install -y docker.io 

apt-get install -y kubelet kubeadm kubectl kubernetes-cni
```

### 使用kubeadm 初始化 kubernetes 集群

```bash
kubeadm init  --pod-network-cidr=10.244.0.0/16
```

这个时候，kubeadm会初始化各种环境配置，包括部署证书（private ca）和密钥，然后调用docker pull相应的镜像

以下是log输出

```bash
[kubeadm] WARNING: kubeadm is in beta, please do not use it for production clusters.
[init] Using Kubernetes version: v1.6.6
[init] Using Authorization modes: [RBAC]
[preflight] Running pre-flight checks
[certificates] Generated CA certificate and key.
[certificates] Generated API server certificate and key.
[certificates] API Server serving cert is signed for DNS names [c-pc kubernetes kubernetes.default kubernetes.default.svc kubernetes.default.svc.cluster.local] and IPs [10.96.0.1 192.168.217.135]
[certificates] Generated API server kubelet client certificate and key.
[certificates] Generated service account token signing key and public key.
[certificates] Generated front-proxy CA certificate and key.
[certificates] Generated front-proxy client certificate and key.
[certificates] Valid certificates and keys now exist in "/etc/kubernetes/pki"
[kubeconfig] Wrote KubeConfig file to disk: "/etc/kubernetes/admin.conf"
[kubeconfig] Wrote KubeConfig file to disk: "/etc/kubernetes/kubelet.conf"
[kubeconfig] Wrote KubeConfig file to disk: "/etc/kubernetes/controller-manager.conf"
[kubeconfig] Wrote KubeConfig file to disk: "/etc/kubernetes/scheduler.conf"
[apiclient] Created API client, waiting for the control plane to become ready
[apiclient] All control plane components are healthy after 710.502147 seconds
[token] Using token: c64554.15b62d72ae89cbb3
[apiconfig] Created RBAC rules
[addons] Applied essential addon: kube-proxy
[addons] Applied essential addon: kube-dns

Your Kubernetes master has initialized successfully!

To start using your cluster, you need to run (as a regular user):

  mkdir -p $HOME/.kube
  sudo cp -i /etc/kubernetes/admin.conf $HOME/.kube/config
  sudo chown $(id -u):$(id -g) $HOME/.kube/config

You should now deploy a pod network to the cluster.
Run "kubectl apply -f [podnetwork].yaml" with one of the options listed at:
  http://kubernetes.io/docs/admin/addons/

You can now join any number of machines by running the following on each node
as root:

  kubeadm join --token c64554.15b62d72ae89cbb3 192.168.217.135:6443
```

提示：在log看到这句输出的时候，docker就开始pull对应的镜像，比较耗时，耗时取决于你的网速   
\[apiclient\] Created API client, waiting for the control plane to become ready

pull完镜像后，可以看到

### kubectl查看node和pod的状态 <a id="kubectl&#x67E5;&#x770B;node&#x548C;pod&#x7684;&#x72B6;&#x6001;"></a>

根据log提示，需要配置kubectl的配置，

初始化kubectl配置

```bash
mkdir -p $HOME/.kube
sudo cp -i /etc/kubernetes/admin.conf $HOME/.kube/config
sudo chown $(id -u):$(id -g) $HOME/.kube/config
```

需要先执行以上操作，否则会遇到以下错误提示

```text
root@c-pc:~# kubectl get nodes
The connection to the server localhost:8080 was refused - did you specify the right host or port?
```

最新的kubectl链接api server不是通过8080端口连接的，而是通过6443端口，可以在admin.conf查看

```text
...
server: https://192.168.217.135:6443
...
```

### Install network\(Flannel\)

For `flannel` to work correctly, you must pass 

`--pod-network-cidr=10.244.0.0/16` to `kubeadm init`.

Set `/proc/sys/net/bridge/bridge-nf-call-iptables` to `1` by running `sysctl net.bridge.bridge-nf-call-iptables=1` to pass bridged IPv4 traffic to iptables’ chains. This is a requirement for some CNI plugins to work, for more information please see [here](https://kubernetes.io/docs/concepts/cluster-administration/network-plugins/#network-plugin-requirements).

Note that `flannel` works on `amd64`, `arm`, `arm64` and `ppc64le`.

```bash
kubectl apply -f https://raw.githubusercontent.com/coreos/flannel/bc79dd1505b0c8681ece4de4c0d86c5cd2643275/Documentation/kube-flannel.yml
```

For more information about `flannel`, see [the CoreOS flannel repository on GitHub](https://github.com/coreos/flannel)



