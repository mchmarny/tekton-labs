# Simple go test task 

## Setup 

Create a namespace:

```shell
kubectl create ns tekton-simple
```

Apply git repo resource (the repo description):

```shell
kubectl apply -f simple/repo-resource.yaml -n tekton-simple
```

Apply the task (in this case a simple go test):

```shell
kubectl apply -f simple/test-task.yaml -n tekton-simple
```

## Run 

To run the task, apply the task run object connecting the resource and the task:

```shell
kubectl apply -f simple/test-run.yaml -n tekton-simple
```

## Monitor

Check the resulting `test-run-pod` status:

```shell
kubectl get pods -n tekton-simple
```

The pod will be going through a series of stages (e.g. `PodInitializing`, `NotReady`, `Completed`):

```shell
NAME                 READY   STATUS            RESTARTS   AGE
test-run-pod-4sg5s   0/2     PodInitializing   0          11s
```

When done, you can also check the `taskrun` status:

```shell
kubectl get taskrun -n tekton-simple
```

The response will include the final state of the task:

```shell
NAME       SUCCEEDED   REASON      STARTTIME   COMPLETIONTIME
test-run   True        Succeeded   49s         7s
```

You can also check the logs for the entire run using the label:

```shell
kubectl logs -l demo=simple --all-containers -n tekton-simple
```

## CLI 

You can also use the Tekton CLI which allows you to easily define the input args to your task (e.g. repo) and output logs all in one command:

```shell
tkn task start test \
    --inputresource repo=app-repo-resource \
    -n tekton-simple \
    --showlog
```