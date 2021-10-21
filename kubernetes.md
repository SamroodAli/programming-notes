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

# Kubernetes services

1. Cluster IP

Sets up an easy to remember URL to access a pod. Only exposes pods in the cluster.

2. Node Port
Makes a pod accessible from outside the cluster. Usually only used for dev purposes.

3. Load Balancer
Makes a pod accessible from outside the cluster. This is the right way to expose a pod to the outside world.

4. External Name
  Redirects an in-cluser request to a CNAME url. Don't worry about this one for now.

# Creating a node port service

node => node port service port=> pod/container port

```yaml
apiVersion: apps/v1
kind: Service
metadata:
  name: serviceName
spec:
  type: ServiceType example: NodePort
  selector:
    matchLabels:
      someLabelKey:someValue of pods
  ports:
    - name: portName
      protocol: TCP
      port: nodeportservicePort
      targetPort: containerPort
```

Example

```yaml
apiVersion: v1
kind: Service
metadata:
  name: posts-srv
spec:
  selector:
    matchLabel:
      app: posts
  ports:
    - name: posts
      protocol: TCP
      port: 4000
      targetPort: 4000
```
 
We have to specify two ports here.

The target port is the port from the project running in the container. For example a node express server might be listening on port on port 4000. The port before target port is the port for the service type. Here NodePort service exposes ports as well. Usually they are both given the same port
