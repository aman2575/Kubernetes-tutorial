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

ExternalName Services create a domain name alias. Here the Pod
can use the local cluster name , which the Kubernetes db-service DNS server resolves to the public address .

The Pod actually communicates with a component outside of the cluster, but that’s transparent. The domain names it uses are local.



# run the DNS lookup tool to resolve the Service name:
kubectl exec deploy/sleep-1 -- sh -c 'nslookup numbers-api | tail -n 5'

# PG: 78 Con for External Name Service, and reason we use headless service
There’s one important thing to understand about ExternalName Services, for components like databases, which communicate over TCP, but it’s not so simple for HTTP services. HTTP requests include the target host name in a header field, and that won’t match the actual domain from the ExternalName response, so the client call will probably fail.

## Understanding Kubernetes Service resolution

Domain name lookups from Pod containers are resolved by the Kubernetes DNS server. For Kubernetes Services, it returns a cluster IP address or an external domain name.

All communication from the Pod is routed by a network proxy, another internal Kubernetes component. A proxy runs on each node. It maintains
an updated list of endpoints for each Service and routes traffic using a network packet filter from the operating system (IPVS or iptables on Linux).

## Namespaces are a way to logically partition a Kubernetes cluster