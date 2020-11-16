# Simple pipeline 

## Setup 

Create a namespace:

```shell
kubectl create ns tekton-ci
```

Create a service account: 

> Note, you will have to update the [secret.yaml](pipeline/secret.yaml) file with your credentials before applying

```shell
kubectl apply -f secret.yaml -n tekton-ci
```

Next, create a service account:

```shell
kubectl apply -f account.yaml -n tekton-ci
```

Now, apply git repo resource (the repo description):

```shell
kubectl apply -f repo-resource.yaml -n tekton-ci
```

Finally, apply the pipeline task:

```shell
kubectl apply -f build-and-push.yaml -n tekton-ci
```

## Run 

To run the pipeline, apply the task run:

```shell
kubectl apply -f task-run.yaml -n tekton-ci
```

## Monitor

Check the resulting `build-and-push-pod` status:

```shell
kubectl get pods -n tekton-ci
```

The pod will be going through a series of stages (e.g. `PodInitializing`, `NotReady`, `Completed`):

```shell
NAME                       READY   STATUS     RESTARTS   AGE
build-and-push-pod-rz42m   1/2     NotReady   0          23s
```

When done, you can also check the `taskrun` status:

```shell
kubectl get taskrun -n tekton-ci
```

The response will include the final state of the task:

```shell
NAME             SUCCEEDED   REASON      STARTTIME   COMPLETIONTIME
build-and-push   True        Succeeded   49s         7s
```

You can also follow the logs while the pipeline executes:

```shell
kubectl logs -l demo=pipeline --all-containers -n tekton-ci
```

When finished, the resulting image should be in your DockerHub image list

https://hub.docker.com/repository/docker/$DOCKER_USER/tekton-labs

And you should be able to run the image: 

```shell
docker run $DOCKER_USER/tekton-labs:latest
```

To run this as a pipeline, fist apply the pipeline manifest:

```shell
kubectl apply -f pipeline.yaml -n tekton-ci
```

And then apply the test, build, push pipeline run:

```shell
kubectl apply -f pipeline-run.yaml -n tekton-ci
```

And check the `PipelineRun` status:

```shell
kubectl get pipelinerun -n tekton-ci
```

You can still follow the logs while the pipeline executes:

```shell
kubectl logs -l demo=pipeline --all-containers -n tekton-ci --max-log-requests 10
```

## CLI 

You can still run the entire pipeline using the Tekton CLI 

```shell
tkn pipeline start test-build-push-pipeline \
    --resource repo=app-repo-resource \
    --serviceaccount build-bot \
    -n tekton-ci \
    --showlog
```

## Disclaimer

This is my personal project and it does not represent my employer. While I do my best to ensure that everything works, I take no responsibility for issues caused by this code.

## License

This software is released under the [MIT](../LICENSE)