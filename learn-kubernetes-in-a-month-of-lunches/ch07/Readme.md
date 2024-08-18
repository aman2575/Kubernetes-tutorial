### Extending applications with multicontainer Pods

One Pod can host many containers, using the same or different Docker images, Contaiers in the Pod can communicate using the localhost address.
Pg 150

# get the detailed Pod information:
kubectl get pod -l app=sleep -o wide

# show the container names:
kubectl get pod -l app=sleep -o jsonpath='{.items[0].status.containerStatuses[*].name}'

# check the Pod logs—this will fail:
kubectl logs -l app=sleep


# list the container names in the new Pod:
kubectl get pod -l app=sleep -o 
jsonpath='{.items[0].status.containerStatuses[*].name}'
# make a network call between the containers:
kubectl exec deploy/sleep -c sleep -- wget -q -O - localhost:8080
# check the server container logs:
kubectl logs -l app=sleep -c server

# create a Service targeting the server container port:
kubectl expose -f sleep/sleep-with-server.yaml --type LoadBalancer -- port 8020 --target-port 8080


pg 156,157


# wait for the containers to start:
kubectl wait --for=condition=ContainersReady pod -l app=timecheck,version=v2


## A volume mount overwrites a directory from the image, if it already exists.

Pg 172