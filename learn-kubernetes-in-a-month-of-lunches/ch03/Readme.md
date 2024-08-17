## Connecting Pods over the network with Services
# Services allow pods to communicate using a fixed domain name.
Pods communicate using domain names. DNS lookups are handled by the internal kubernetes DNS server. It returns the IP address of the Service.

Creating a Service effectively registers it with the DNS server, using an IP address that is static for the life of the Service.

The Service is loosely coupled to the Pod using the same label-selector approach that Deployments use. A Service can be the virtual address for zero or more Pods. The Pod does not have a Service, so it cannot be reached with a DNS name


## 3.2 Routing traffic between Pods
The default type of Service in Kubernetes is called ClusterIP.


It creates a clusterwide
IP address that Pods on any node can access. The IP address works only within the
cluster, so ClusterIP Services are useful only for communicating between Pods. 


### How can we allow one cluster pod to communicate to another cluster pod? and restrict them??

## Commands to check pod is ready 
kubectl wait --for=condition=Ready pod -l app=numbers-web


A LoadBalancer Service integrates with an external load balancer, which sends traffic to the cluster. The Service sends the traffic to a Pod, using the same label-selector mechanism to identify a target Pod.

## What is the difference between port and targetPort


NodePort Services have each node listening on the
Service port. There’s no external load balancer, so
traffic is routed directly to the cluster nodes.

The Service works in a similar way to a LoadBalancer Service
once the traffic is inside the cluster: any node can receive a
request and direct it to Node 3, which is running the Pod.

## Why do we need loadbalancer why can't we just use nodeport service instead?
NodePort Services don’t have the flexibility of LoadBalancer Services because you
need a different port for each Service, your nodes need to be publicly accessible,
and you don’t achieve load-balancing across a multinode cluster.


### 3.4 Routing traffic outside Kubernetes Cluster