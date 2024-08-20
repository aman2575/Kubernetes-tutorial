### Kubernetes as a service developer workflow

Buildpacks use the same concept as multistage Dockerfiles: running the build
tools inside a container to compile the app and then packaging the compiled app on
top of another container image that has the application runtime.

## Context
A context defines the connection details for a Kubernetes cluster and sets the default namespace to use in kubectl commands.