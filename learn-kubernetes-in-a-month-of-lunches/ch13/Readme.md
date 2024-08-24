### Centralized logs with Fluentd and Elasicsearch

- Fluentd is the collector component, and it has some noce integration with kubernets.
- Elasticsearch is the storage component and can run either as Pods in the cluster or as an external service.
- This model assumes your application logs are written to the container standard output streams so kubernetes can find them.

# 13.1 How Kubernetes stores log entries

- Kubernetes collects log entries from the container runtime and stores them as files on the node running the container.
- Nodes store log entries exactly as they come from the container, using filenames that include the namespace, Pod, and container names.

#  Logging in Kubernetes uses a collector like Fluentd to read the log files from the node.
- Container logs are stored as files
on the node running the Pod.
- A log collector Pod runs on every node and mounts the host path where the log files
are stored, so it can read them. We’ll use Fluentd to collect and process logs
- The collector forwards logs to a central store, which has a search UI.
We’ll use Elasticsearch for storage and Kibana for the UI.