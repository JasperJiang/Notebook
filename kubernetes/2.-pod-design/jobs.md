# Jobs

A job creates one or more pods and ensures that a specified number of them successfully terminate. When the pods successfully complete, the jobchecks for the successful completions. When a desired number of successful completions are achieved, then the job will get completed. Deleting a Job will cleanup the pods created by it. The Job object will start a new Pod if the first pod fails or is deleted.

### Simple Job to run echo “Hello World”

Create Simple Job from following configuration file.

```yaml
apiVersion: batch/v1
kind: Job
metadata:
  name: nginx
spec:
  template:
    metadata:
      name: nginx
      labels:
        app: job
    spec:
      containers:
      - name: nginx
        image: nginx:1.9.1
        command: ["/bin/sh"]
        args: ["-c", "echo Hello World"]
      restartPolicy: Never
```



This configuration file create the job which require 1 completeion. It checks whether the pod successfully rxecuted command. Once pod complete its execution successfully job get completed.

Deploy a job from above Yaml file.

```bash
$ kubectl apply -f configs/simplejob.yaml
```

* Get list of Jobs

```bash
$ kubectl get job
```



* Get the list of pod which are part of above Job.

```text
$ kubectl get po --show-all
```

```text
NAME          READY     STATUS      RESTARTS   AGE
nginx-wzb68   0/1       Completed   0          56s
```

* Check the logs of abve pod you will see output of that job.

```text
$ kubectl logs -l app=job
```

```text
Hello World
```

#### Cron Jobs. <a id="cron-jobs"></a>

Cron job is type of the time based jobs. Cron Jobs managed job by running them at specific time or running them periodically. In `spec.schedule` you have to mention the time condition for job at which this job get scheduled and get completed.

#### Cron Job to run something at 10 PM everyday. <a id="cron-job-to-run-something-at-10-pm-everyday"></a>

* Create a cron job configuration from follwoing file.

```yaml
apiVersion: batch/v1beta1
kind: CronJob
metadata:
  name: cron-job-demo
spec:
  schedule: "0 22 * * *"
  jobTemplate:
    spec:
      template:
        spec:
          containers:
          - name: demo
            image: nginx:1.9.1
            command: ["/bin/sh"]
            args: ["-c", "echo Time is 10PM"]
          restartPolicy: OnFailure
```

In above configuration file we have specified that `schedule: "0 22 * * *"` which mean our job will be scheduled at 0 minute and 22 hours. It follows 24 hour clock so the job gets scheduled at 10 PM everyday.

* Deploy the cron job.

```text
$ kubectl apply -f configs/1-cronjob.yaml
```

* Get list of Cron job.

```text
$ kubectl get cronjob
```

```text
NAME            SCHEDULE     SUSPEND   ACTIVE    LAST SCHEDULE   AGE
cron-job-demo   0 22 * * *   False     0         <none>          1s
```

#### Parallel Jobs and Job Completion <a id="parallel-jobs-and-job-completion"></a>

* Create a job which specifies the number of replicas those should run in parallel to complete the job. Here we have specified 2 replicas and 10 completions that mean job will complete the 10 executions.

```yaml
apiVersion: batch/v1
kind: Job
metadata:
  name: nginx-parallel
spec:
  completions: 10
  parallelism: 2
  template:
    metadata:
      name: nginx
    spec:
      containers:
      - name: nginx
        image: nginx:1.9.1
        command: ["/bin/sh"]
        args: ["-c", "echo Hello World"]
      restartPolicy: OnFailure
```

Deploy this job.

```text
$ kubectl apply -f configs/2-paralleljob.yaml 
```

Check the status of the jobs.

```text
$ kubectl get jobs 
```

```text
NAME               DESIRED   SUCCESSFUL   AGE
nginx-parallel     10        0            8s
```

Check the status of Pods.

```text
$ kubectl get po
```

```text
NAME                   READY     STATUS              RESTARTS   AGE
nginx-parallel-f955r   0/1       ContainerCreating   0          1s
nginx-parallel-pbt9s   0/1       ContainerCreating   0          1s
```

When the job completly get executed check the status of the jobs.

```text
$ kubectl get jobs
```

```text
NAME             DESIRED   SUCCESSFUL   AGE
nginx            1         1            8m
nginx-parallel   10        10           2m
```

### Delete Jobs and Cron Job.

```text
$ kubectl delete jobs nginx-parallel
$ kubectl delete cronjob cron-job-demo
```

