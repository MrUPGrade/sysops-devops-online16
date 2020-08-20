# sysops-devops-online16

Examples for the SysOps/DevOps online meetup #16

This examples were tested on minikube with Kubernetes version 1.17.4

# Argo

Install

```shell script
kubectl create ns argo
kubectl config set-context --current --namespace=argo
kubectl apply -n argo -f https://raw.githubusercontent.com/argoproj/argo/stable/manifests/quick-start-postgres.yaml
```

For UI
```shell script
kubectl -n argo port-forward deployment/argo-server 2746:2746
```

Secret for dockerhub
```shell script
kubectl create secret generic dockerhub \
    --from-file=config.json=${HOME}/.docker/config.json 
```

Run
```shell script
argo submit -p buildnumber=3  ci/fastapi-playground.yaml
```


# Tekton

Install

```shell script
kubectl apply --filename https://storage.googleapis.com/tekton-releases/pipeline/latest/release.yaml
kubectl config set-context --current --namespace=tekton-pipelines
```

Secret for dockerhub
```shell script
kubectl create secret docker-registry regcred \
                    --docker-server=<your-registry-server> \
                    --docker-username=<your-name> \
                    --docker-password=<your-pword> \
                    --docker-email=<your-email>
```

Setup
```shell script
kubectl apply -f tekton/serviceaccount.yaml
kubectl apply -f tekton/resources.yaml 
kubectl apply -f tekton/tasks.yaml 
kubectl apply -f tekton/pipeline.yaml
```

Run
```shell script
kubectl apply -f tekton/pipelinerun.yaml
```
# IceCI

Installation
```shell script
kubectl create ns iceci
kubectl config set-context --current --namespace=iceci
kubectl apply -f https://raw.githubusercontent.com/IceCI/IceCI/master/manifests/crds.yaml
kubectl apply -f https://raw.githubusercontent.com/IceCI/IceCI/master/manifests/minikube.yaml
```

Secret for dockerhub
```shell script
kubectl create secret generic dockerhub \
    --from-file=config.json=${HOME}/.docker/config.json 
kubectl label secrets --overwrite dockerhub app.kubernetes.io/managed-by=iceci iceci.io/secret-type=docker
```


Run
```shell script
kubectl apply -f iceci/gitpipeline.yaml 

kubectl apply -f iceci/multiple-steps.yaml
```