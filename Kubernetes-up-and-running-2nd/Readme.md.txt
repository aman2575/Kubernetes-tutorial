 Cluster Components:

	 Kubernetes Proxy:  responsible for routing network traffic to load-balanced
 services in the Kubernetes cluster
			 -	the proxy must be present on every
 node in the cluster. Kubernetes has an API object named DaemonSet

		kubectl get daemonSets --namespace=kube-system kube-proxy

	Kubernetes DNS:   
	- provides naming and discovery for the
 services that are defined in the cluster.
	- kubectl get deployments --namespace=kube-system core-dns
	- There is also a Kubernetes service that performs load balancing for the DNS server:
		- kubectl get services --namespace=kube-system core-dns

	Kubernetes UI: 
	- kubectl get deployments --namespace=kube-system kubernetes-dashboard
	- The dashboard also has a service that performs load balancing for the dashboard:
		- kubectl get services --namespace=kube-system kubernetes-dashboard
	- You can use kubectl proxy to access this UI.
		$ kubectl proxy

Chapter 4:

Namespaces: 
	-  Kubernetes uses namespaces to organize objects in the cluster. 
	- By default, the kubectl command line tool interacts with the default namespace.

Contexts:
	- create a context with a different default namespace for your kubectl commands using:
		- $ kubectl config set-context my-context --namespace=mystuff
 	- This creates a new context, but it doesn’t actually start using it yet. 	- To use this newly created context, you can run:
		- $ kubectl config use-context my-context

Viewing Kubernetes API Objects:
	 this command will extract and print the IP address of the specified Pod:
	-  $ kubectl get pods my-pod -o jsonpath --template={.status.podIP}

Creating, Updating, and Destroying Kubernetes Objects:
	-  The apply command also records the history of previous configurations in an annotation within the object.
		- $ kubectl apply -f myobj.yaml view-last-applied

Labeling and Annotating Objects:
	-  to add the color=red label to a Pod named bar, you can run:
		- $ kubectl label pods bar color=red
		- By default, label and annotate will not let you overwrite an existing label. To do this, you need to add the --overwrite flag.
	-  If you want to remove a label, you can use the <label-name>- syntax:
		- $ kubectl label pods bar color-
	- If you don’t have bash or some other terminal available within your container, you can always attach to the running process:
		-  kubectl attach -it <pod-name>
	-  copy files to and from a container using the cp command:
		-  $ kubectl cp <pod-name>:</path/to/remote/file> </path/to/local/file>
	-  cluster is using resources
		- kubectl top nodes
		- kubectl top pods
		-  will show all Pods and their resource usage.
	- Cabin mobile app for viewing Kubernetes Cluster

What Are cgroups?
A control group (cgroup) is a Linux kernel feature that limits, accounts for, and isolates the resource usage (CPU, memory, disk I/O, network, and so on) of a collection of processes


Chapter 5
Pods: 
	- Each container within a Pod runs in its own cgroup, but they share a number of
 Linux namespaces.

System V IPC or POSIX ? ? ?
Creating a Pod:
	- $ kubectl run kuard --generator=run-pod/v1 --image=gcr.io/kuar-demo/kuard-amd64:blue
Getting More Info with Logs:
	- Previous Pods logs
		-  $ kubectl logs kuard --previous
	- Copying Files to and from Containers
		-  $ kubectl cp <pod-name>:/captures/capture3.txt ./capture3.txt
		-  $ kubectl cp $HOME/config.txt <pod-name>:/config.txt
	- Readiness Probe:
		-  Containers that fail liveness checks are restarted. 
		-   Containers that fail readiness checks are removed from service load balancers.
		-   Combining the readiness and liveness probes helps ensure only healthy containers are running within the cluster.

Chapter 6
Labels and Annotations:
	-  Labels are key/value pairs that can be attached to Kubernetes objects such as Pods and ReplicaSets.
	- Labels provide the foundation for grouping objects
	-  Annotations, on the other hand, provide a storage mechanism that resembles labels:
		-  annotations are key/value pairs designed to hold nonidentifying information that can be leveraged by tools and libraries

Labels:
	-  Labels have simple syntax. They are key/value pairs, where both the key and value are represented by strings.
	-  Label keys can be broken down into two parts: 
		- an optional prefix and a name, separated by a slash. 
			- The prefix, if specified, must be a DNS sub-domain with a 253-character limit. 
			- The key name is required and must be shorter
			- Names must also start and end with an alphanumeric character and permit the use of dashes (-), underscores (_), and dots (.) between characters than 63 characters. 
	-  Label values are strings with a maximum length of 63 characters. 
Applying Labels: 
	-  $ kubectl run alpaca-prod \
  		--image=gcr.io/kuar-demo/kuard-amd64:blue \
  		--replicas=2 \
  		--labels="ver=1,app=alpaca,env=prod"
	- $ kubectl run alpaca-test \
 		 --image=gcr.io/kuar-demo/kuard-amd64:green \
  	         --replicas=1 \
 		 --labels="ver=2,app=alpaca,env=test"
	-  $ kubectl get deployments --show-labels
	-  $ kubectl get deployments -L canary

Modifying Labels:
	-  $ kubectl label deployments alpaca-test "canary=true"

Label Selectors:
	- $ kubectl get pods --show-labels
	- $ kubectl get pods --selector="ver=2"
	- If we specify two selectors separated by a comma, only the objects that satisfy both will be returned. This is a logical AND operation:
 		- $ kubectl get pods --selector="app=bandicoot,ver=2"
	- we ask for all Pods where the app label is set to alpaca or bandicoot (which will be all six Pods):
 		- $ kubectl get pods --selector="app in (alpaca,bandicoot)"
	- $ kubectl get deployments --selector="canary"

Chapter 7
Service Discovery:
	-  This readiness check is a way for an overloaded or sick server to signal to the system that it doesn’t want to receive traffic anymore. This is a great way to implement graceful shutdown. The server can signal that it no longer wants traffic, wait until existing connections are closed, and then cleanly exit.
	- NodePort:
		-  If your cluster is in the cloud someplace, you can use SSH tunneling with something like this:
 			- $ ssh <node> -L 8080:localhost:32711
Endpoints:
	- Endpoints object that contains the IP addresses for that service: