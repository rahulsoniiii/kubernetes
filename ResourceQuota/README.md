### **Resource Limits and Requests Documentation**

In Kubernetes, resource limits and requests define the CPU and memory allocation for containers. Here's a quick overview of the provided values and their units:

- `limits.cpu: "1"`: Sets the maximum CPU limit to 1 CPU core.
- `limits.memory: "400Mi"`: Sets the maximum memory limit to 400 Mebibytes (MiB).
- `requests.cpu: "500m"`: Specifies a minimum CPU requirement of 500 milliCPU units.
- `requests.memory: "1Gi"`: Specifies a minimum memory requirement of 1 Gibibyte (GiB).

Units used in Kubernetes for resource limits and requests:

- CPU units:
  - MilliCPU (m): 1/1000th of a CPU core
  - CPU (no suffix): 1 CPU core

- Memory units:
  - Mebibyte (Mi): 1024^2 bytes
  - Gibibyte (Gi): 1024^3 bytes

These values ensure that your containers have sufficient resources to run effectively. Adjust them based on your application's requirements and available cluster resources.

This documentation provides a concise explanation of resource limits and requests in Kubernetes, including the units used for CPU and memory specifications.