## ConfigMap and Secret objects like other resources.
- By applying YAML, using kubectl, either with create commands 
- They don't do anything they're just storage units intended for small amounts for data. . Those storage units can be loaded into a Pod, becoming
part of the container environment, so the application in the container can read the
data.


# Command to Pod to be ready
kubectl wait --for=condition=Ready pod -l app=sleep

# ConfigMap are seprate resourcesm which can be attached to zero or more Pods
- Pods load ConfigMaps into the container environment as environment variables or files.
- ConfigMaps can be used for app-specific settings or shared settings used in many Pods. ConfigMap data is read-only; Pods can't alter it.

## 4.2 Storing and using configuration files in ConfigMaps

# show the default config file:
kubectl exec deploy/todo-web -- sh -c 'ls -l /app/app*.json'

# show the config file in the volume mount:
kubectl exec deploy/todo-web -- sh -c 'ls -l /app/config/*.json'