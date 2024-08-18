### Storing Data with volumes, mounts and claims


# 5.1 How Kubernetes builds the container filesystem
- Each container in a pod has its own filesystem, which is constructed by kubernets
- The filesystem can be built from  multiple sources -at a minimum, the layer from the container image and the writable layer for the containers

# Write a file inside the container:
kubectl exec deploy/sleep -- sh -c 'echo ch05 > /file.txt; ls /*.txt



# kill all processes in the container, causing a Pod restart:
kubectl exec -it deploy/sleep -- killall5

# look for the file you wrote—it won’t be there:
kubectl exec deploy/sleep -- ls /*.txt



# EmptyDir
An empty directory sounds like the least useful piece of storage you can imagine, but
actually it has a lot of uses because it has the same life cycle as the Pod. Any data stored
in an EmptyDir volume remains in the Pod between restarts, so replacement containers can access data written by their predecessors.

- You can use EmptyDir volumes for any applications that use the filesystem for temporary storage—maybe your app calls an API, which takes a few seconds to respond, and the response is valid for a long time.

- EmptyDir volumes only share the life cycle of the Pod, so if the Pod is replaced, then the new Pod starts with, well, an empty directory.

- Chaching files in an EmptyDir volume means the cache survives Pod restarts.


# read the file in the volume:
kubectl exec deploy/sleep -- cat /data/file.txt

# find the app URL from your LoadBalancer:
kubectl get svc pi-proxy -o jsonpath='http://{.status.loadBalancer.ingress[0].*}:8080/?dp=30000'

# HostPath volume
- Maps to directory on the node's disk
- A HostPath volume is defined in the pod spec like any volume and mounted into the conainer filesystem
- The data in the volume is actually stored in a directory on the host node's filesystem. If the pod replaced, it will have access to the files, but only if it runs on the same node.
- This method extends the durability of the data beyond the life cycle of the Pod to the life cycle of the node’s disk, provided replacement Pods always run on the same node.

- The obvious problem with HostPath volumes is that they don't make sense in a cluster with more than one node.

- Danger! Mounting a HostPath can give you complete access to the data on the node.

- HostPath volumes are a good way to start with stateful apps
- They are useful in real-world application, too, but only when your apps are using state for temporary storage.

# 5.3 Storing clusterwide data with persisten volume and claims

- Distributed storage gives your Pod access to data from any node, but it needs platform support.
- A PVC needs to be bound before a Pod can use it. If you deploy a Pod that references an unbound PVC, the POD will stay in the Pending state until the PVC is bound,


# 5.4 Dynamic Volume Provisioning and storage classes

Storage classes provide a lot of flexibility. You create them as standard Kubernetes resources, and in the spec, you define exactly how the storage class works with the following three fields:

    - provisioner — the component that creates PVs on demand. Different platforms have different provisioners, for        example,  the provisioner in the default AKS
    storage class integrates with Azure Files to create new file shares.
    - reclaimPolicy — defines what to do with dynamically created volumes when the claim is deleted. The underlying volume can be deleted, too, or it can be retained.
    - volumeBindingMode — determines whether the PV is created as soon as the PVC is created or not until a Pod that uses the PVC is created.

# How to use storage drivers for storing secrets and using as volume 