# LoadBalancer

When we have cluster on cloud which support external load balancers. Then by setting the `type` field to `LoadBalancer` will provision a load balancer for our Service.   
The actual creation of the load balancer happens asynchronously, and information about the provisioned balancer will be published in the Service’s `.status.loadBalancer` field.

### Labs <a id="labs"></a>

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

Create following yaml for Load Balancer to create service to above Deployment.

```yaml
apiVersion: v1
kind: Service
metadata:
  name: nginx-lb
  labels:
    app: nginx
spec:
  type: LoadBalancer
  ports:
  - port: 80
    targetPort: 80
    protocol: TCP
  selector:
    app: nginx
```

#### Deploy Loadbalancer from above yaml. <a id="deploy-loadbalancer-from-above-yaml"></a>

```text
$ kubectl apply -f configs/2-loadbalancer.yaml 
```

#### Get the list of services. <a id="get-the-list-of-services"></a>

```text
$ kubectl get svc -l app=nginx
```

```text
NAME       TYPE           CLUSTER-IP       EXTERNAL-IP                                                               PORT(S)        AGE
nginx-lb   LoadBalancer   100.65.24.180    a062d9f78c47511e881d002820176578-1454043559.us-west-2.elb.amazonaws.com   80:31765/TCP   48s
```

With this external IP of Load Balancer you can access the nginx application.

#### Clean up <a id="clean-up"></a>

```text
$ kubectl delete svc nginx-lb
$ kubectl delete deploy nginx-deployment
```

