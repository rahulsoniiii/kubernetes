**Taints and Tolerations Documentation**

Taints and tolerations are mechanisms in Kubernetes that allow nodes to repel or attract pods based on certain criteria. Taints are applied to nodes, while tolerations are applied to pods, enabling fine-grained control over pod placement in the cluster.

**Practical Example:**

To demonstrate the use of taints and tolerations, follow these steps:

1. Apply a taint to a node:

```bash
kubectl taint nodes <node-name> key=value:taint-effect
```

Replace `<node-name>` with the name of the target node and specify the taint key-value pair and the taint effect. The taint effect can be one of the following:

- `NoSchedule`: Pods without matching tolerations will not be scheduled on the tainted node.
- `PreferNoSchedule`: The scheduler will try to avoid placing pods without matching tolerations on the tainted node, but it's not guaranteed.
- `NoExecute`: Existing pods without matching tolerations will be evicted from the tainted node.

2. Create a pod with tolerations:

```
apiVersion: v1
kind: Pod
metadata:
  name: my-pod
spec:
  containers:
    - name: my-container
      image: nginx:latest
  tolerations:
    - key: key
      operator: Exists
      effect: NoSchedule
```

The `tolerations` section in the pod configuration allows the pod to tolerate the specified taint.

3. Apply the pod configuration:

```
kubectl apply -f pod.yaml
```

**Taint and Toleration Effects:**

- `NoSchedule`: Pods without matching tolerations will not be scheduled on nodes with this taint.
- `PreferNoSchedule`: The scheduler will try to avoid placing pods without matching tolerations on nodes with this taint, but it's not guaranteed.
- `NoExecute`: Existing pods without matching tolerations will be evicted from nodes with this taint.

**Common Taint and Tolerations Commands:**

- List all nodes and their taints:

```
kubectl describe nodes
```

- Add a taint to a node:

```
kubectl taint nodes <node-name> key=value:taint-effect
```

- Remove a taint from a node:

```
kubectl taint nodes <node-name> key-
```

- Describe a pod to view its tolerations:

```
kubectl describe pod <pod-name>
```

**Theory:**

- Taints are applied to nodes and repel pods without matching tolerations.
- Tolerations are applied to pods and allow them to tolerate specific taints.
- Taints and tolerations provide fine-grained control over pod placement in the cluster.
- The three taint effects are `NoSchedule`, `PreferNoSchedule`, and `NoExecute`, each affecting pod scheduling and eviction differently.