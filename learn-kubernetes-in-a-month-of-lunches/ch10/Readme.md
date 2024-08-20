### Packaging and Managing apps with Helm


-- You can use HELM to group a set of Kubernetes YAML files into one artifact and share that on a public or private repo.

## What Helm adds to Kubernetes

- An application package in Helm is called a chart
- charts can be developed and deployed locally or published to
a repository.
- When you install a chart, that’s called a release; every release has a name,
and you can install multiple instances of the same chart in your cluster as separate,
named releases
- Charts contain Kubernetes YAML manifests, and the manifests typically contain
parameterized values so users can install the same chart with different configuration
settings
- Charts are packaged as compressed archives, named with the chart name and version. The archive contains a folder, which is the name of the chart.
- The chart file contains metadata, including name, version, and description.
- The values file contains default values for any parameterized settings in the Kubernetes manifests.
- The templates folder contains Kubernetes manifests, whichcan have parameterized values.


## Packaging your own apps with Helm
One of the big benefits of Helm over standard manifest deployments is that you can
run multiple instances of the same app from a single chart. You can’t do that with
kubectl because the manifests contain resource names that need to be unique. If you
deploy the same set of YAML multiple times, Kubernetes will just update the same
resources. If you template all the unique parts of the spec—like resource names and
label selectors—then you can run many copies of the same app with Helm

# 3 stag process
    - zip
    - upload the archive to a server
    - Update the repo index to add the new chart

