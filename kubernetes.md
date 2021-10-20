# Kubernetes

Kubernetes is a tool for running a bunch of different containers

We give it a configuration to describe how we want our containers to run and interact with each other.

We use docker to manage individual containers and kubernetes to manage multiple containers.


|Docker World   | K8s World   |
|---|---|
| docker ps   | kubectl get pods  |
| docker exec -it [containerid][cmd]|kubectl exec -itt [pod_name][cmd]|
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
Service makes networking between pods a lot easier and intuitive.
