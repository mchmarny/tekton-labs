# tekton-labs

Collection of Tekton demos

## Setup 

Install Tekton and Tekton Pipelines into the cluster:

> More info [here](https://tekton.dev/docs/getting-started/)

```shell
kubectl apply -f https://storage.googleapis.com/tekton-releases/pipeline/latest/release.yaml
```

## Demos

* [Simple](./simple) - Simple go test on a tit repo
* [Pipeline](./pipeline) - Test, build, push to DockerHub pipeline example 


## Disclaimer

This is my personal project and it does not represent my employer. While I do my best to ensure that everything works, I take no responsibility for issues caused by this code.

## License

This software is released under the [MIT](./LICENSE)