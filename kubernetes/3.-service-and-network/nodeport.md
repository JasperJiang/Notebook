# NodePort

### Labs <a id="labs"></a>

With Nodeport inspite of having a cluster-internal IP, it expose the service on a port on each node of the cluster \(the same port on each node\). We can access the service on any `<NodeIP>:NodePort`. If you set the type field to “NodePort”, the Kubernetes master will allocate a random port from a range \(default range is 30000-32767\), and each Node will proxy that port \(the same port number on every Node\) into your Service. You can specify the perticuler nodeport also.

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

#### Create a Nodeport service by using following yaml file. <a id="create-a-nodeport-service-by-using-following-yaml-file"></a>

This service will expose the above created `nginx-deployment`deployment to the the port of node. With this port and and master-IP we can access our application from outside of cluster

```yaml
apiVersion: v1
kind: Service
metadata:
  name: nginx-nodesvc
  labels:
    app: nginx
spec:
  type: NodePort
  ports:
  - port: 80
    nodePort: 31009
    protocol: TCP
  selector:
    app: nginx
```

In above configuration file we have exposed this service to the 31009 port of node. If we dont specify the perticuler port then random port will be allocated.

#### Deploy Nodeport service. <a id="deploy-nodeport-service"></a>

```text
$ kubectl apply -f configs/2-nodeport.yaml 
```

#### Get the information of of the services present and their ports. <a id="get-the-information-of-of-the-services-present-and-their-ports"></a>

```text
$ kubectl get svc
```

```text
NAME             CLUSTER-IP   EXTERNAL-IP   PORT(S)        AGE
kubernetes       10.0.0.1     <none>        443/TCP        1h
nginx-nodesvc    10.0.0.55    <nodes>       80:31009/TCP   1m
```

Above created nginx is exposed at 31009 Nodeport by this nginx-nodesvc. So with the node's IP and this port we can aceess the Application from outside world.

#### Get the Node IP. <a id="get-the-node-ip"></a>

```text
$ kubectl get node -o wide
```

```text
NAME                                          STATUS    ROLES     AGE       VERSION   EXTERNAL-IP      OS-IMAGE                      KERNEL-VERSION   CONTAINER-RUNTIME
ip-172-20-42-226.us-west-2.compute.internal   Ready     master    1d        v1.10.3   54.201.214.249   Debian GNU/Linux 8 (jessie)   4.4.121-k8s      docker://17.3.2
ip-172-20-46-21.us-west-2.compute.internal    Ready     node      1d        v1.10.3   34.217.40.145    Debian GNU/Linux 8 (jessie)   4.4.121-k8s      docker://17.3.2
ip-172-20-61-64.us-west-2.compute.internal    Ready     node      1d        v1.10.3   35.162.251.247   Debian GNU/Linux 8 (jessie)   4.4.121-k8s      docker://17.3.2
```

#### Acccess the nginx pod via Nodeport and Node IP. <a id="acccess-the-nginx-pod-via-nodeport-and-node-ip"></a>

```bash
$ curl http://34.217.40.145:31009/
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
<h1>Welcome to nginx!</h1>
<p>If you see this page, the nginx web server is successfully installed and
working. Further configuration is required.</p>

<p>For online documentation and support please refer to
<a href="http://nginx.org/">nginx.org</a>.<br/>
Commercial support is available at
<a href="http://nginx.com/">nginx.com</a>.</p>

<p><em>Thank you for using nginx.</em></p>
</body>
</html>
```

#### Delete Service and Pod. <a id="delete-service-and-pod"></a>

```text
$ kubectl delete svc nginx-nodesvc
$ kubectl delete deploy nginx-deployment
```

