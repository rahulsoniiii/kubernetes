**ConfigMap Documentation**

ConfigMap is a Kubernetes resource used to store configuration data that can be consumed by pods or other Kubernetes objects. It provides a way to separate the configuration from the container images, making it easier to manage and update configurations without modifying the application code.

**Creating ConfigMap Using Imperative Approach**

1. Create a ConfigMap named `my-config` with key-value pairs:

```
kubectl create configmap my-config --from-literal=key1=value1 --from-literal=key2=value2
```

2. View the created ConfigMap:

```
kubectl get configmap my-config
```

3. Describe the details of the ConfigMap:

```
kubectl describe configmap my-config
```

4. Edit the ConfigMap:

```
kubectl edit configmap my-config
```

**Creating ConfigMap Using Declarative Approach**

1. Create a YAML file named `configmap.yaml` with the following contents:

```
apiVersion: v1
kind: ConfigMap
metadata:
  name: my-config
data:
  key1: value1
  key2: value2
```

2. Apply the ConfigMap using the YAML file:

```bash
kubectl apply -f configmap.yaml
```

3. View the created ConfigMap:

```bash
kubectl get configmap my-config
```

4. Describe the details of the ConfigMap:

```bash
kubectl describe configmap my-config
```

5. Edit the ConfigMap:

```bash
kubectl edit configmap my-config
```

**Accessing ConfigMap Data in Pods**

To access the data stored in a ConfigMap within a pod, you can use environment variables or volume mounts.

1. Using environment variables:

Add the following environment variables to the pod spec in your manifest file:

```
apiVersion: v1
kind: Pod
metadata:
  name: my-pod
spec:
  containers:
    - name: my-app
      image: my-image
      env:
        - name: KEY1
          valueFrom:
            configMapKeyRef:
              name: my-config
              key: key1
        - name: KEY2
          valueFrom:
            configMapKeyRef:
              name: my-config
              key: key2
```

2. Using volume mounts:

Add the following volume and volume mount to the pod spec in your manifest file:

```
apiVersion: v1
kind: Pod
metadata:
  name: my-pod
spec:
  containers:
    - name: my-app
      image: my-image
      volumeMounts:
        - name: config-volume
          mountPath: /etc/config
  volumes:
    - name: config-volume
      configMap:
        name: my-config
```

**Additional ConfigMap Commands**

- Delete a ConfigMap:
  ```
  kubectl delete configmap my-config
  ```

- Export the ConfigMap to a YAML file:
  ```
  kubectl get configmap my-config -o yaml > configmap-export.yaml
  ```

- Import a ConfigMap from a YAML file:
  ```
  kubectl apply -f configmap-import.yaml
  ```