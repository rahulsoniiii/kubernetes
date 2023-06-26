**DaemonSet Documentation**

A DaemonSet is a Kubernetes resource that ensures a specific pod runs on each node within a cluster. It is primarily used for deploying background services, logging agents, monitoring tools, or any other system-level service that needs to be present on every node.

**Practical Example:**

To create a DaemonSet, follow these steps:

1. Create a YAML file (e.g., `daemonset.yaml`) and add the following content:

```
apiVersion: apps/v1
kind: DaemonSet
metadata:
  name: my-daemonset
spec:
  selector:
    matchLabels:
      app: my-app
  template:
    metadata:
      labels:
        app: my-app
    spec:
      containers:
        - name: my-container
          image: nginx:latest
```

2. Apply the DaemonSet configuration using the `kubectl apply` command:

```bash
kubectl apply -f daemonset.yaml
```

3. Verify that the DaemonSet pods are running on each node:

```bash
kubectl get pods -l app=my-app
```

**Note:** DaemonSet pods will be automatically created and scheduled on each node in the cluster.

**Important Directories:**

DaemonSets don't have specific directories associated with them. Instead, they rely on the standard Kubernetes pod directories, which include:

- `/etc`: Contains system-level configuration files.
- `/var`: Stores variable data, such as logs and caches.
- `/run`: Holds runtime data, such as PID files and sockets.

**Theory:**

- DaemonSets are used for deploying system-level services that need to run on every node in the cluster.
- They ensure that a single pod of the specified configuration is running on each node.
- DaemonSets are managed by the Kubernetes control plane, which monitors node changes and schedules or terminates pods as necessary.
- Each DaemonSet pod is unique and tied to a specific node. If a new node is added, a new pod will be automatically created on that node.
- DaemonSets are useful for running cluster-wide services like log collectors, monitoring agents, or network proxies.