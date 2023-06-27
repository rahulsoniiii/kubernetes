**Pod Scheduling in Kubernetes: Automatic and Manual**

In Kubernetes, pod scheduling refers to the process of assigning pods to nodes in the cluster. There are two primary methods of pod scheduling: automatic scheduling and manual scheduling. This document explains both approaches and includes practical examples.

**Automatic Scheduling:**

Automatic scheduling is the default behavior in Kubernetes. The Kubernetes scheduler determines the best node for each pod based on resource requirements, affinity/anti-affinity rules, node selectors, and other factors.

To use automatic scheduling, follow these steps:

1. Create a pod configuration file (`pod.yaml`) specifying the pod's resource requirements and other specifications.

   Example `pod.yaml`:

   ```
   apiVersion: v1
   kind: Pod
   metadata:
     name: my-pod
   spec:
     containers:
       - name: my-container
         image: nginx:latest
         resources:
           requests:
             cpu: "500m"
             memory: "256Mi"
           limits:
             cpu: "1"
             memory: "512Mi"
   ```

2. Apply the pod configuration using the `kubectl apply` command:

   ```
   kubectl apply -f pod.yaml
   ```

   The Kubernetes scheduler will analyze the pod's requirements and available resources to find a suitable node for scheduling the pod.

**Manual Scheduling:**

Manual scheduling allows you to explicitly specify the node where a pod should be scheduled. This is useful when you want fine-grained control over pod placement.

There are two common approaches for manual scheduling: using the `nodeName` field and using `NodeSelector`.

**Manual Scheduling with `nodeName`:**

To manually schedule a pod to a specific node using the `nodeName` field:

1. Update the pod configuration (`pod.yaml`) to include the `nodeName` field with the desired node's name:

   ```
   apiVersion: v1
   kind: Pod
   metadata:
     name: my-pod
   spec:
     nodeName: my-node
     containers:
       - name: my-container
         image: nginx:latest
   ```

   Replace `my-node` with the name of the target node.

2. Apply the pod configuration using `kubectl apply`:

   ```
   kubectl apply -f pod.yaml
   ```

The pod will be scheduled only on the specified node.

**Manual Scheduling with `NodeSelector`:**

To manually schedule a pod to nodes based on labels using `NodeSelector`:

1. Update the pod configuration (`pod.yaml`) to include the `nodeSelector` field with the desired label selector:

   ```
   apiVersion: v1
   kind: Pod
   metadata:
     name: my-pod
   spec:
     containers:
       - name: my-container
         image: nginx:latest
     nodeSelector:
       disktype: ssd
   ```

   This example selects nodes with the label `disktype: ssd`.

2. Apply the pod configuration using `kubectl apply`:

   ```
   kubectl apply -f pod.yaml
   ```

The pod will be scheduled on nodes that have the specified label.

**Common Commands:**

- List all nodes in the cluster:

  ```
  kubectl get nodes
  ```

- Describe a specific node:

  ```
  kubectl describe node <node-name>
  ```

These commands are useful for inspecting nodes and gathering information about the cluster's resources.