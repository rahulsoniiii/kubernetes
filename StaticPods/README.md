### **Static Pod Documentation**

Static Pods in Kubernetes are a special type of pod that is managed directly by the kubelet daemon on a specific node, without the involvement of the control plane components. They provide a way to run pods directly on a node, allowing for node-level control and customization. Here's a concise documentation on static pods:

#### Static Pod Manifest Directory
The static pod manifest directory is `/etc/kubernetes/manifests`. The kubelet on a node monitors this directory for any changes and automatically manages the static pods based on the manifest files present in this directory. Place the pod manifest file in this directory to have it managed as a static pod.

#### Kubelet Configuration File
The kubelet configuration file, which specifies the configuration for the kubelet on a node, is located at `/var/lib/kubelet/config.yml`. This file contains various settings and parameters that control the behavior of the kubelet, including the directory path for the static pod manifest files.

#### Creating a Static Pod
To create a static pod, follow these steps:

1. Create a manifest file, for example, `my-static-pod.yaml`, defining the pod specifications.

```
apiVersion: v1
kind: Pod
metadata:
  name: my-static-pod
spec:
  containers:
    - name: my-container
      image: nginx:latest
      ports:
        - containerPort: 80
```

2. Place the `my-static-pod.yaml` file in the `/etc/kubernetes/manifests` directory on the target node.

3. The kubelet running on the node will detect the manifest file and automatically create and manage the static pod based on the provided specifications.

#### Important Note
It's important to note that static pods are managed directly by the kubelet daemon on a specific node. Each static pod requires its own manifest file in the `/etc/kubernetes/manifests` directory, and the kubelet only manages the pods defined in individual manifest files.

There is only one static object in Kubernetes: Static Pods. There are no static deployments or any other static objects in the Kubernetes ecosystem.

It's crucial to remember that there can only be one static pod per manifest file.

Static pods offer a straightforward and node-centric approach to running pods on Kubernetes. By placing the manifest file in the static pod manifest directory, the kubelet takes care of running and managing the static pod, providing flexibility and customization at the node level.