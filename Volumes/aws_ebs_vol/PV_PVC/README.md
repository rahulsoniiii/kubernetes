### **Persistent Volumes (PV) and Persistent Volume Claims (PVC) Documentation**

PV and PVC are crucial components for managing persistent storage in Kubernetes.

**Persistent Volumes (PV)**

PVs represent physical storage in the cluster and have a lifecycle independent of any pod. They are provisioned by cluster administrators and can be dynamically or statically provisioned. Some key points about PVs:

- A single PV can be bound to only one PVC at a time.
- If a PVC requests a smaller capacity than the available PV, the extra storage remains unused and cannot be utilized by other PVCs.
- When multiple PVs are available, the choice of PV for binding to a PVC is determined by the access modes, storage class, and selectors specified in the PVC's configuration.
- The access modes define the level of concurrent access allowed to the PV (e.g., Read/Write Once, Read/Write Many, Read Only).
- The storage class defines the provisioner and other parameters for dynamic provisioning.
- Selectors in PVC configuration can be used to match specific PVs based on labels or other criteria.

**Persistent Volume Claims (PVC)**

PVCs are requests for storage made by pods. They specify the desired storage capacity, access mode, and other requirements. Some key points about PVCs:

- A PVC can only be bound to a single PV at a time.
- If a PVC requests a larger capacity than the available PVs, it will remain in a "Pending" state until a suitable PV becomes available.
- PVCs are created by users or controllers, and they can dynamically provision PVs based on storage class definitions.
- When a PVC is created, Kubernetes searches for an appropriate PV based on the specified access mode, storage class, and selectors.
- PVCs prefer PVs with equal or larger capacity than requested to avoid wasting storage.
- If multiple PVs satisfy the criteria, Kubernetes selects the PV based on other factors like priority, age, or labeling.

**Deployment and Service Example**

Here's an example of a Kubernetes deployment file that includes a service definition:

```yaml
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
          image: nginx:latest
          ports:
            - containerPort: 80

---
apiVersion: v1
kind: Service
metadata:
  name: my-service
spec:
  selector:
    app: my-app
  ports:
    - protocol: TCP
      port: 80
      targetPort: 80
  type: NodePort
```

In this example:

- The Deployment creates three replicas of a containerized application based on the NGINX image.
- The Service exposes the deployed pods by selecting them based on the label `app: my-app`.
- The Service is of type `NodePort`, which allows access to the service from outside the cluster.
- The service is mapped to port 80 on the nodes, and traffic is forwarded to port 80 on the pods.

To apply this configuration to your Kubernetes cluster, you can use the following command:

```bash
kubectl apply -f deployment.yaml
```

After applying the configuration, you can forward the service port to your local machine using the following command:

```bash
kubectl port-forward svc/my-service 80:80
```

Now you can access the service from your local machine by opening a web browser and visiting `http://localhost:80`.

Please note that the actual YAML file should be saved as `deployment.yaml` or any

 other name you prefer.

### Access Modes and PersistentVolumeReclaimPolicy in Kubernetes

**Access Mode:**

Access Mode in Kubernetes specifies how a Persistent Volume (PV) can be accessed by Pods. There are three access modes:

1. ReadWriteOnce (RWO): Allows the volume to be mounted as read-write by a single Node.

2. ReadOnlyMany (ROX): Allows the volume to be mounted as read-only by multiple Nodes.

3. ReadWriteMany (RWX): Allows the volume to be mounted as read-write by multiple Nodes.

**PersistentVolumeReclaimPolicy:**

PersistentVolumeReclaimPolicy determines what happens to the Persistent Volume (PV) when the associated Persistent Volume Claim (PVC) is deleted. There are three reclaim policies:

1. Retain: Retains the PV and its data even after the PVC is deleted. Manual cleanup of the PV is required.

2. Delete: Deletes the PV and its data automatically when the PVC is deleted.

3. Recycle (Deprecated): Performs a basic cleanup by deleting the contents of the PV when the PVC is deleted. This policy is deprecated and no longer recommended.