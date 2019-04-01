# ClusterIP

### Labs <a id="labs"></a>

Kubernetes Pods are mortal. They may die and be restarted anywhere in the cluster at any time.  
When restarted, a Pod may be assigned a different IP address. This leads to a problem: if some set of Pods \(let’s call them backends\) provides functionality to other Pods \(let’s call them frontends\) inside the Kubernetes cluster, how do those frontends find out and keep track of which backends are in that set? Here, Services come to the rescue.

**A Kubernetes Service** create a logical connection to the set of Pods and a policy by which to access them. The set of Pods targeted by a Service is \(usually\) determined by a Label Selector.

This service uses a cluster-internal IP only. This is the default service type. Choosing this value means that you want this service to be reachable only from inside of the cluster.

#### Create A deployment from following configuration. <a id="create-a-deployment-from-following-configuration"></a>

```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: nginx-deployment
  labels:
    app: nginx
spec:
  replicas: 1
  selector:
    matchLabels:
      app: nginx
  template:
    metadata:
      labels:
        app: nginx
    spec:
      containers:
      - name: nginx-ctr
        image: nginx:1.15.4
        ports:
        - containerPort: 80
```

#### Deploy above Deployment. <a id="deploy-above-deployment"></a>

```text
$ kubectl apply -f configs/1-deploy.yaml
```

#### Create a `clusterIP` service for above created deployment. <a id="create-a-clusterip-service-for-above-created-deployment"></a>

```yaml
apiVersion: v1
kind: Service
metadata:
  name: nginx-svc
  labels:
    app: nginx
spec:
  type: ClusterIP
  ports:
  - port: 80
    targetPort: 80
    protocol: TCP
  selector:
    app: nginx
```

#### Deploy above service. <a id="deploy-above-service"></a>

```text
$ kubectl apply -f configs/2-clusterIP-svc.yaml
```

#### Get the list of services <a id="get-the-list-of-services"></a>

```text
$ kubectl get svc
```

```text
NAME         CLUSTER-IP   EXTERNAL-IP   PORT(S)   AGE
kubernetes   10.0.0.1     <none>        443/TCP   1h
nginx-svc    10.0.0.134   <none>        80/TCP    8s
```

#### Verify the DNS service via nslookup <a id="verify-the-dns-service-via-nslookup"></a>

* Run another deployement nginx and check the dnslookup by service name.

```text
$ kubectl run demo -it --rm --image=nginx:1.9.1 /bin/sh
```

* Inside Shell terminal execute following commands

```text
$ apt update
```

```text
$ apt-get install dnsutils
```

```text
$ nslookup nginx-svc
```

```text
Server:     10.0.0.10
Address:    10.0.0.10#53

Name:   nginx-svc.default.svc.cluster.local
Address: 10.0.0.134
```

Within the same namespace service can be accessed by the other service/pods with its DNS name.

#### Delete Deployment and service. <a id="delete-deployment-and-service"></a>

```text
$ kubectl delete svc nginx-svc
$ kubectl delete deploy nginx-deployment
```

#### PodIP <a id="podip"></a>

Each Pod is assigned a unique IP address. Every container in a Pod shares the network namespace, including the IP address and network ports. Containers inside a Pod can communicate with one another using localhost. If pod get restarted same IP cant be retained, it will get new IP.

* Create A deployment from following configuration.

```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: nginx-deployment
  labels:
    app: nginx
spec:
  replicas: 1
  selector:
    matchLabels:
      app: nginx
  template:
    metadata:
      labels:
        app: nginx
    spec:
      containers:
      - name: nginx-ctr
        image: nginx:1.15.4
        ports:
        - containerPort: 80
```

* Deploy above Deployment.

```text
$ kubectl apply -f configs/1-deploy.yaml
```

* Get the list of the pods.

```text
$ kubectl get pod
```

```text
NAME                               READY     STATUS    RESTARTS   AGE
nginx-deployment-8778fb659-kh2k8   1/1       Running   0          5m
```

* Get the detailed information of the pod.

```text
$ kubectl get pod -l app=nginx -o wide
```

```text
NAME                               READY     STATUS    RESTARTS   AGE       IP               NODE
nginx-deployment-8778fb659-kh2k8   1/1       Running   0          4m        100.116.17.184   ip-172-20-51-24.us-west-2.compute.internal
```

Here You can see podIP.

* Lets access that PodIP from another container.

```text
$ kubectl run demo -it --rm --image=python /bin/sh
```

```text
If you don't see a command prompt, try pressing enter.
# curl 100.116.17.184
```

```markup
<!DOCTYPE html>
<html>
<head>
<title>Welcome to nginx!</title>
<style>
    body {
        width: 35em;
        margin: 0 auto;
        font-family: Tahoma, Verdana, Arial, sans-serif;
    }
</style>
</head>
<body>
.
.
```

#### Clean Up. <a id="clean-up"></a>

```text
$ kubectl delete deploy nginx-deployment
```

