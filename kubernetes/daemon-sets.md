# Daemon Sets

## Concept

A `DaemonSet` responsible for running an instance of a specific pod on all or selected nodes in a cluster. When any new node is added to the cluster then DaemonSet creates pods on each added node and When any node is removed from cluster then it removes pods which are running on that specific node.

## Lab

### Get the list of Nodes available in your cluster.

```bash
$ kubectl get nodes
```

```bash
NAME      STATUS    ROLES     AGE       VERSION
master    Ready     master    9m        v1.9.3
node1     Ready     <none>    8m        v1.9.3
node2     Ready     <none>    8m        v1.9.3
node3     Ready     <none>    8m        v1.9.3
```

In this cluster `Master` is the master node and by default scheduling is disabled on the master node. While remaining 3 nodes are the worker nodes, Which are schedulable.

So when we deploy the Daemon-set it deploy a pod on all the nodes\(by default\), or you can specify by using the `.spec.template.spec.nodeSelector` so the pods will be scheduled on specific nodes.In this cluster, by default DaemonSet will create 3 pods, running on each node except the master node.

### Let's create the DaemonSet from following configuration.

```yaml
apiVersion: apps/v1
kind: DaemonSet
metadata:
  name: nginx-ds
spec: 
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

### Deploy the DaemonSet from above configuration file.

```bash
$ kubectl apply -f configs/1-daemonset.yaml
```

### Get the list and status of DaemonSet.

```bash
$ kubectl get ds -o wide
```

```bash
NAME       DESIRED   CURRENT   READY   UP-TO-DATE   AVAILABLE   NODE SELECTOR   AGE   CONTAINERS   IMAGES        SELECTOR
nginx-ds   3         3         3       3            3           <none>          9s    nginx        nginx:1.9.1   app=nginx
```

### Get list of pods. We can verify that on each node one pod is deployed.

```bash
$ kubectl get po -l app=nginx -o wide
```

```bash
NAME                           READY   STATUS        RESTARTS   AGE  IP                NODE
nginx-ds-qckx6                 1/1     Running       0          3m   100.105.249.106   node1
nginx-ds-wfnwd                 1/1     Running       0          3m   100.125.108.201   node2
nginx-ds-xqxxs                 1/1     Running       0          3m   100.105.140.106   node3
```

### Run DaemonSet Pods on specific Nodes.

When you want to deploy the `Daemon Set` only to a specific set of nodes instead of deploying on all the nodes in cluster, you can use a node selector to specify nodes for the `Daemon Set`. For doing so, you need to have labeled your nodes accordingly.

### Label the nodes. lets label node 1 and 2 as disk=spinning

```bash
$ kubectl label nodes node1 node2 disk=spinning
```

### Lets Modify above DaemonSet Configuration file as shown below.

```yaml
apiVersion: apps/v1
kind: DaemonSet
metadata:
  name: nginx-ds-node
spec: 
  selector:
    matchLabels:
      app: nginx1
  template:
    metadata:
      labels:
        app: nginx1
    spec:
      containers:
      - name: nginx
        image: nginx:1.9.1
        ports:
        - containerPort: 80
      nodeSelector:
        disk: spinning    
```

### Deploy the reconfigured DaemonSet.

```bash
$ kubectl apply -f configs/2-ds-node.yaml
```

### Get the list and status of DaemonSet.

```bash
$ kubectl get ds -o wide
```

```bash
NAME            DESIRED   CURRENT   READY     UP-TO-DATE   AVAILABLE   NODE SELECTOR   AGE       CONTAINERS   IMAGES        SELECTOR
nginx-ds        3         3         3         3            3           <none>          17m       nginx        nginx:1.9.1   app=nginx
nginx-ds-node   2         2         2         2            2           disk=spinning   35s       nginx        nginx:1.9.1   app=nginx1
```

As we have given labels `disk=spinning` to only 2 nodes. Hence 2 pods are created.

### Get the list of Pods. We can see that pods have been deployed only on two nodes .i.e node1, node2.

```text
$ kubectl get pods -l app=niginx1 -o wide
```

```bash
NAME                  READY     STATUS    RESTARTS   AGE       IP                NODE
nginx-ds-node-5jchz   1/1       Running   0          1m        100.105.14.108    node1
nginx-ds-node-fpvj5   1/1       Running   0          1m        100.125.108.197   node2
```

### Clean Up.

```bash
$ kubectl delete ds nginx-ds nginx-ds-node
```

