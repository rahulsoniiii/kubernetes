# Node Affinity and Anti-Affinity in Kubernetes

## Introduction
Node affinity and anti-affinity are Kubernetes features that allow you to influence the scheduling of pods onto specific nodes based on node labels. They help control pod placement, optimize resource allocation, and enhance fault tolerance within the cluster.

### Affinity and Anti-Affinity
- **Affinity**: Node affinity is a concept that allows you to define rules for scheduling pods onto specific nodes based on node labels. It specifies the preferences for pod placement.
- **Anti-Affinity**: Node anti-affinity is the opposite of node affinity. It enables you to specify rules for spreading pods across different nodes based on node labels, ensuring high availability and fault tolerance.
Anti-affinity in Kubernetes allows you to schedule pods in a way that prevents them from being co-located on the same node or with pods that have matching labels. 

### Scheduling pods on nodes with specific labels

```
apiVersion: v1
kind: Pod
metadata:
  name: my-pod
spec:
  containers:
    - name: my-container
      image: my-image
  affinity:
    nodeAffinity:
      requiredDuringSchedulingIgnoredDuringExecution:
        nodeSelectorTerms:
          - matchExpressions:
              - key: app
                operator: In
                values:
                  - my-app
``` 

### Scheduling deployment in all nodes except the mentioned one

```
apiVersion: apps/v1
kind: Deployment
metadata:
  name: my-deployment
spec:
  replicas: 3
  selector:
    matchLabels:
      app: my-app
  template:
    metadata:
      labels:
        app: my-app
    spec:
      affinity:
        nodeAffinity:
          requiredDuringSchedulingIgnoredDuringExecution:
            nodeSelectorTerms:
              - matchExpressions:
                  - key: node-name
                    operator: DoesNotExist
      containers:
        - name: my-container
          image: my-image
```


## Using Node Affinity and Anti-Affinity
### Attribute: `requiredDuringSchedulingIgnoredDuringExecution`
The `requiredDuringSchedulingIgnoredDuringExecution` attribute is used to specify that a pod must be scheduled onto a node that matches the defined criteria. If no such node is available, the pod will remain unscheduled.

### Attribute: `preferredDuringSchedulingIgnoredDuringExecution`
The `preferredDuringSchedulingIgnoredDuringExecution` attribute is used to specify a preference for scheduling pods onto nodes that match the defined criteria. If no such node is available, the scheduler will still attempt to schedule the pod onto other nodes.

## Scheduling Pods using 1 File Method
In the 1 file method, you can schedule replicas of a pod on all matching nodes within the cluster. Here's an example manifest:

```
apiVersion: v1
kind: Pod
metadata:
  name: my-pod
spec:
  replicas: 3
  template:
    metadata:
      labels:
        app: my-app
    spec:
      containers:
        - name: my-container
          image: my-image
  affinity:
    podAntiAffinity:
      requiredDuringSchedulingIgnoredDuringExecution:
        - labelSelector:
            matchExpressions:
              - key: app
                operator: In
                values:
                  - my-app
          topologyKey: "kubernetes.io/hostname"

```

This manifest will create 3 replicas of the pod and schedule them on all available nodes.

## Scheduling Pods using 2 File Method
In the 2 file method, you can divide the replicas of a pod equally across all available nodes within the cluster. Here's an example manifest:

**Deployment Manifest (`deployment.yaml`):**
```
apiVersion: apps/v1
kind: Deployment
metadata:
  name: my-deployment
spec:
  replicas: 3
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
          image: my-image
```

**Affinity Manifest (`affinity.yaml`):**
```
apiVersion: v1
kind: Pod
metadata:
  name: my-pod
spec:
  affinity:
    podAntiAffinity:
      requiredDuringSchedulingIgnoredDuringExecution:
        - labelSelector:
            matchExpressions:
              - key: app
                operator: In
                values:
                  - my-app
          topologyKey: "kubernetes.io/hostname"
```

The `deployment.yaml` creates a deployment with 3 replicas of the pod, and the `affinity.yaml` defines the anti-affinity rule to evenly distribute the replicas across the nodes.

## Schedule Pod on Every Node Except One
To schedule a pod on every node except one, you can use the following manifest:

```
apiVersion: v1
kind: Pod
metadata:
  name: my-pod
spec:
  affinity:
    nodeAffinity:
      requiredDuringSchedulingIgnoredDuringExecution:
        nodeSelectorTerms:
          - matchExpressions:
              - key: node-name


                operator: DoesNotExist
```

In this manifest, the pod is scheduled on nodes where the label `node-name` does not exist, effectively excluding one specific node from the scheduling process.

## Note:- 
1. *Two-file method:* Replicas are distributed among nodes based on the anti-affinity rule.
2. *Single-file method:* All replicas are scheduled on each available node.