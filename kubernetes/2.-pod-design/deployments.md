# Deployments

## Concept



Deployments are intended to replace Replication Controllers. They provide the same replication functions with the help of Replica Sets and Deployments also have the ability to rollout changes and roll them back if necessary.

* Deployment is usually we make new release and update in kubernetes cluster.
* So we describe a desired state in a Deployment object, and the Deployment controller changes the actual state to the desired state at a controlled rate.
* We define Deployments to create new ReplicaSets, or to remove existing Deployments and adopt all their resources with new Deployments.
* Following are cases can be done on deployments.
  * Create a Deployment to rollout a ReplicaSet:
    * ReplicaSet creates Pods in the background. Check the status of the rollout to see if it succeeds or not.
    * We can see deployment status by :`kubectl rollout status deployment/nginx-deploy`
  * Declare the new state of the Pods :
    * We can update the current pod state i.e. update deployment updating the image of container
    * A new ReplicaSet is created and the Deployment manages moving the Pods from the old ReplicaSet to the new one at a controlled rate.
    * Each new ReplicaSet updates the revision of the Deployment.
    * View view revision history of deployment : `kubectl rollout history deploy/nginx-deploy`
  * Rollback to an earlier Deployment:
    * If current version isn't working as expected we can update revision if the current state of the Deployment is not stable.
    * Each rollback updates the revision of the Deployment.
    * Rollback to previous version: `kubectl rollout undo deployment/nginx-deploy`
    * We can rollback to specific versions too : `kubectl rollout undo deployment/nginx-deployment --to-revision=2`
  * Scale up the Deployment to facilitate more load.
    * We can scale the deployment when we need to.
    * To add 5 replicas : `kubectl scale deployment nginx-deployment --replicas=5`
  * We can Pause the Deployment to apply multiple fixes to its PodTemplateSpec and then resume it to start a new rollout.
  * Use the status of the Deployment as an indicator that a rollout has stuck.
  * Clean up older ReplicaSets that you don’t
    * We can use `.spec.revisionHistoryLimit` to set limit to revision history of replicaset we can to keep.

## Labs

### Create Deployment from configuration file.

```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: nginx-deploy
spec:
  replicas: 3
  selector:
    matchLabels:
      app: nginx
  template:
    metadata:
      labels:
        app: nginx
    spec:
      containers:
      - name: nginx
        image: nginx:1.9.1
        ports:
        - containerPort: 80
```

### Deploy a Deployment from configuration file.

```bash
$ kubectl apply -f configs/1-deployment.yaml
```

### Get the list of current Deployments.

```bash
$ kubectl get deployment
```

```bash
NAME           DESIRED   CURRENT   UP-TO-DATE   AVAILABLE   AGE
nginx-deploy   3         3         3            3           24s
```

### Scaling the Deployments.

Scaling Up the Deployment to 5 Replicas.

```bash
$ kubectl scale deployment nginx-deploy --replicas=5
```

Check the Deployments and Replicas.

```bash
$ kubectl get deploy
```

```text
NAME           DESIRED   CURRENT   UP-TO-DATE   AVAILABLE   AGE
nginx-deploy   5         5         5            5           1m
```

## Deployment Strategies.



In Kubernetes there are a few different ways to release an application, it is necessary to choose the right strategy to make your infrastructure reliable during an application update.

**Ramped/RollingUpdate \[Release a new version on a rolling update fashion, one after the other\]**

The ramped deployment strategy consists of slowly rolling out a version of an application by replacing instances one after the other until all the instances are rolled out. It usually follows the following process: with a pool of version A behind a load balancer, one instance of version B is deployed. When the service is ready to accept traffic, the instance is added to the pool. Then, one instance of version A is removed from the pool and shut down.

Depending on the system taking care of the ramped deployment, you can tweak the following parameters to increase the deployment time:

Parallelism, max batch size: Number of concurrent instances to roll out.  
Max surge: How many instances to add in addition of the current amount.  
Max unavailable: Number of unavailable instances during the rolling update procedure.

Create a deployment configurations as follow.

```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: my-app
  labels:
    app: my-app
spec:
  replicas: 10
  selector:
    matchLabels:
      app: my-app
  template:
    metadata:
      labels:
        app: my-app
        version: v1.0.0
    spec:
      containers:
      - name: my-app
        image: nginx:1.9.1
        ports:
        - name: http
          containerPort: 80
        env:
        - name: VERSION
          value: v1.0.0
        livenessProbe:
          httpGet:
            path: /
            port: 80
          initialDelaySeconds: 5
          periodSeconds: 5
        readinessProbe:
          httpGet:
            path: /
            port: 80
          periodSeconds: 5
```

Deploy the first application:

```bash
$ kubectl apply -f configs/ramped/appv1.yaml
```

Test if the deployment was successful:

```bash
$ kubectl get pod -l app=my-app
```

To see the deployment in action, open a new terminal and run the following command:

```bash
$ watch kubectl get po
```

Create a deployment with RollingUpdate strategy

```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: my-app
  labels:
    app: my-app
spec:
  replicas: 10
  strategy:
    type: RollingUpdate
    rollingUpdate:
      maxSurge: 1
      maxUnavailable: 0
  selector:
    matchLabels:
      app: my-app
  template:
    metadata:
      labels:
        app: my-app
        version: v2.0.0
    spec:
      containers:
      - name: my-app
        image: teamcloudyuga/app
        ports:
        - name: http
          containerPort: 80
        env:
        - name: VERSION
          value: v2.0.0
        livenessProbe:
          httpGet:
            path: /
            port: 80
          initialDelaySeconds: 5
          periodSeconds: 5
        readinessProbe:
          httpGet:
            path: /
            port: 80
          # Intial delay is set to a high value to have a better
          # visibility of the ramped deployment
          initialDelaySeconds: 15
          periodSeconds: 5
```

Then go to the previous terminal and deploy second application:

```bash
$ kubectl apply -f configs/ramped/appv2.yaml
```

Take a look at the pods.

```bash
$ watch kubectl get po
```

If you can also pause the rollout :

```bash
$ kubectl rollout pause deploy my-app
```

Then if you are satisfy with the result, rollout:

```bash
$ kubectl rollout resume deploy my-app
```

In case you discover some issue with the new version, you can undo the rollout:

```bash
$ kubectl rollout undo deploy my-app
```

Delete the deployment of application.

```bash
$ kubectl delete all -l app=my-app
```



**Recreate**

The recreate strategy is a dummy deployment which consists of shutting down version A then deploying version B after version A is turned off. This technique implies downtime of the service that depends on both shutdown and boot duration of the application

Create a configurations for the deployment:

```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: my-app
  labels:
    app: my-app
spec:
  replicas: 5
  strategy:
    type: Recreate
  selector:
    matchLabels:
      app: my-app
  template:
    metadata:
      labels:
        app: my-app
        version: v1.0.0
    spec:
      containers:
      - name: my-app
        image: nginx:1.9.1
        ports:
        - name: http
          containerPort: 80
        env:
        - name: VERSION
          value: v1.0.0
        livenessProbe:
          httpGet:
            path: /
            port: 80
          initialDelaySeconds: 5
          periodSeconds: 5
        readinessProbe:
          httpGet:
            path: /
            port: 80
          periodSeconds: 5
```

Deploy the first deployment and service of application:

```bash
$ kubectl apply -f configs/recreate/app-v1.yaml
```

To see the deployment in action, open a new terminal and run the following command:

```bash
$ watch kubectl get po
```

Create deployment configuration for version 2 of application.

```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: my-app
  labels:
    app: my-app
spec:
  replicas: 5
  strategy:
    type: Recreate
 
  selector:
    matchLabels:
      app: my-app
  template:
    metadata:
      labels:
        app: my-app
        version: v2.0.0
    spec:
      containers:
      - name: my-app
        image: nginx:stable
        ports:
        - name: http
          containerPort: 80
        env:
        - name: VERSION
          value: v2.0.0
        livenessProbe:
          httpGet:
            path: /
            port: 80
          initialDelaySeconds: 5
          periodSeconds: 5
        readinessProbe:
          httpGet:
            path: /
            port: 80
          periodSeconds: 5
```

Then deploy version 2 of the application:

```bash
$ kubectl apply -f configs/recreate/app-v2.yaml
```

In the terminal where we have opened the `watch kubectl getpo` you can see older pods are going down and newer pods are coming up.

Delete the deployment and service of application.

```bash
$ kubectl delete all -l app=my-app
```

### Delete Deployment.

```text
$ kubectl delete deploy nginx-deploy
```

