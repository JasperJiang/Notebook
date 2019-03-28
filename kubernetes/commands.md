# 1. core concept

### Check Version

```bash
kubectl version
```

```bash
Client Version: version.Info{Major:"1", Minor:"10", GitVersion:"v1.10.0",
GitCommit:"fc32d2f3698e36b93322a3465f63a14e9f0eaead", GitTreeState:"clean", BuildDate:"2018-03-26T16:55:54Z",
GoVersion:"go1.9.3", Compiler:"gc", Platform:"linux/amd64"}
Server Version: version.Info{Major:"1", Minor:"10", GitVersion:"v1.10.0", 
GitCommit:"fc32d2f3698e36b93322a3465f63a14e9f0eaead", GitTreeState:"clean", BuildDate:"2018-03-26T16:44:10Z",
GoVersion:"go1.9.3", Compiler:"gc", Platform:"linux/amd64"}
```

### Check Cluster Info

```bash
kubectl cluster-info
```

```bash
Kubernetes master is running at https://206.189.110.98:6443
KubeDNS is running at https://206.189.110.98:6443/api/v1/namespaces/kube-system/services/kube-dns:dns/proxy

To further debug and diagnose cluster problems, use 'kubectl cluster-info dump'.
```

### Get Cluster Health

```bash
kubectl get componentstatus
```

```bash
NAME                 STATUS    MESSAGE              ERROR
scheduler            Healthy   ok
controller-manager   Healthy   ok
etcd-0               Healthy   {"health": "true"}
```

### Get APIServer End-Points

```bash
kubectl api-versions
```

```bash
dmissionregistration.k8s.io/v1beta1apiextensions.k8s.io/v1beta1
apiregistration.k8s.io/v1apiregistration.k8s.io/v1beta1apps/v1apps/v1beta1
apps/v1beta2
authentication.k8s.io/v1
authentication.k8s.io/v1beta1authorization.k8s.io/v1
authorization.k8s.io/v1beta1
autoscaling/v1autoscaling/v2beta1
batch/v1
batch/v1beta1
certificates.k8s.io/v1beta1
crd.projectcalico.org/v1
events.k8s.io/v1beta1
extensions/v1beta1
networking.k8s.io/v1
policy/v1beta1
rbac.authorization.k8s.io/v1
rbac.authorization.k8s.io/v1beta1
storage.k8s.io/v1
storage.k8s.io/v1beta1
v1
```

### Get Nodes

```bash
kubectl get nodes
```

```bash
NAME                       STATUS    ROLES     AGE       VERSION
k8s-master-node-trainee1   Ready     master    2h        v1.10.0
k8s-worker-node-trainee1   Ready     <none>    2h        v1.10.0
```

### List Namespaces

```bash
kubectl get namespaces
```

```bash
NAME          STATUS    AGE
default       Active    2h
kube-public   Active    2h
kube-system   Active    2h
```

### Kube Config

* Kubectl uses kubeconfig file for connecting to cluster.
* Kubeconfig file usually consist of
  * clusters
  * users
  * contexts

Sample example of config:

```text
apiVersion: v1
clusters:
- cluster:
    certificate-authority: fake-ca-file
    server: https://1.2.3.4
  name: development
- cluster:
    insecure-skip-tls-verify: true
    server: https://5.6.7.8
  name: scratch
 
contexts:
- context:
    cluster: development
    namespace: frontend
    user: developer
  name: dev-frontend
- context:
    cluster: development
    namespace: storage
    user: developer
  name: dev-storage
- context:
    cluster: scratch
    namespace: default
    user: experimenter
  name: exp-scratch
 
current-context: ""
kind: Config
preferences: {}
 
users:
- name: developer
  user:
    client-certificate: fake-cert-file
    client-key: fake-key-file
- name: experimenter
  user:
    password: some-password
    username: exp
```

#### **Clusters**

Cluster refer to the entire kubernetes cluser, it has the address to connect to cluster and key to connect to it.

#### **Users**

User here no such object in kubernetes, user refers to the certificate and key file that has access to cluster.  
We can use RBAC rules to regulate which key has access to which part of cluster.

#### **Context**

* Each context is a triple \(cluster, user, namespace\).
* For example, the dev-frontend context says, Use the credentials of the developer user to access the frontend namespace of the development cluster.

### kubectl get vs kubectl describe.

* `kubectl get` is used to list the resources.
  * E.g. `kubectl get pods`, `kubectl get pod podname`
* `kubectl describe` is used to get detailed information about the resource
  * E.g. : `kubectl describe pod podname`
* Kubectl get has various output formats like `wide`, `yaml`, `json`

### kubectl create vs apply

Imperative way \(Create\)  
Declarative way \(Apply\)

Difference b/w imperative vs declarative.

