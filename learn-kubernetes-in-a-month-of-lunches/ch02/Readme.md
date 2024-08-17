—a managed Kubernetes cluster won’t give you control of Docker or containerd

What is the use of wget and curl

You can read file contents to check that configuration settings are being applied correctly, run DNS queries to verify that services are resolving as expected, and ping endpoints to test the network.


## You can run commands in Pods that are managed by a Deployment without knowing the Pod name, and you can view the logs of all Pods that match a label selector

#  Pod we created from the Deployment YAML file:
kubectl exec deploy/hello-kiamol-4 -- sh -c 'wget -O - http://localhost > /dev/null'

# and check logs that Pod's logs:
kubectl logs --tail=1 -l app=hello-kiamol-4


# Copy a file into pod container
kubectl exec hello-kiamol -- cat /usr/share/nginx/html/index.html > ./kiamol/ch02/index.html



## Connecting Pods over the network with Services
# Services allow pods to communicate using a fixed domain name.
Pods communicate using domain names. DNS lookups are handled by the internal kubernetes DNS server. It returns the IP address of the Service.

Creating a Service effectively registers it with the DNS server, using an IP address that is static for the life of the Service.

The Service is loosely coupled to the Pod using the same label-selector approach that Deployments use. A Service can be the virtual address for zero or more Pods. The Pod does not have a Service, so it cannot be reached with a DNS name