# Kubernetes

Kubernetes is a tool for running a bunch of different containers

We give it a configuration to describe how we want our containers to run and interact with each other.

We use docker to manage individual containers and kubernetes to manage multiple containers.

|Docker World | K8s World   |
|---|---|
| docker ps   | kubectl get pods  |
| docker exec -it [containerid][cmd]|kubectl exec [-it] [pod_name] -- [cmd]|
|docker logs [container id]| kubectl logs [pod_name]|
|docker kill [containerId]|kubectl delete pod [pod_name]|
|docker-compose up --build | kubectl apply -f [config gile name]|
||kubectl describe pod [pod_name]|

# K8s objects
# Kubernetes Pods

Pods are the most basic units available in kubernetes. Pods run containers inside them. Their life is the same as the container's life inside.

# Deployment
Deployments are another objects of K8s. They are a higher level abstraction for managing pods. They make sure the required count of pods are always running. If one fails, it will redeploy another.

# Service
Service makes networking between pods a lot easier and intuitive. Pods reach out to service and service reaches out to other pods.

# Creating a pod

```yml
# Specify the version of kubernetes Object collection.
apiVersion: v1
# The object we want to create is a pod
kind: Pod
# Data about the pod
metadata:
  name: podName
# Data about the object, here Pod 
spec:
# We are running containers in the Pod
  containers:
  # Takes in an array of objects describing the container
    - name: containerName
      image: local or dockerhub image, give version if local
      # if local, give this to avoid imagePull Policy issue
      imagePullPolicy: Never

```

If ran into a imageErr/imagePullOff or something of this sort, do steps below in the SAME TERMINAL SESSION.
1. run eval $(minikube docker-env)
2. and build the image

# Creating a deployment

```yml
# specify the collection of kubernetes objects, for deployment=> apps/v1
apiVersion: apps/v1
# specify the kind of Object we are creating
kind: Deployment
# Meta data about the deployment
metadata:
  name: deploymentName
# Deployment data => what we are deploying
spec:
  # No of pods
  replicas: 1
  # selector => we have to tell deployment what pods it needs to manage
  selector:
    # pods that match labels
    matchLabels:
      anyKey: anyValue # example app: posts
  # Create the actual pods 
  template:
  # metadata about the pods
    metadata:
      labels:
      # labels for the pods which will be matched in selector
        sameKeyAsAbove: sameValueAsAbove
    # data about the pods
    spec:
      containers:
        - name: podName
          image: local or dockerhub image, give version if local
          imagePullPolicy: Never

```

# Kubernetes deployments common commands

1. To get deployments
```bash
kubectl get deployments
``` 

2. To get info on deployments

```bash
kubectl describe deployment deployment-name
```
3. To create a deployment

```bash
kubectl apply -f pathToConfig.yaml

```

4. To delete a deployment

```bash
kubectl delete deployment deployment-name
```

# Updates in kubernetes deployments

1. Deployment pods must not specify any version or are using the 'latest' tag.
2. Make updates to codebase.
3. Build the image
4. Push the image to docker hub
5. Run the rollout and restart comman

```
kubectl rollout restart deployment [deployment_name]

```
