### **StatefulSet Documentation**

StatefulSet is a Kubernetes resource used for managing stateful applications. It provides guarantees for ordering and uniqueness of pods, making it suitable for applications that require stable network identities, stable storage, and ordered scaling.

#### **StatefulSet vs Deployment**

| **StatefulSet** | **Deployment** |
|-----------------|----------------|
| Manages stateful applications where each instance has a unique identity and stable network and storage. | Manages stateless applications where each instance is interchangeable and can be scaled horizontally. |
| Pods in a StatefulSet have unique identifiers based on their ordinal index. | Pods in a Deployment have randomly generated names. |
| Scaling a StatefulSet is performed in an ordered manner, one at a time. | Scaling a Deployment can be performed in parallel without specific ordering. |
| Deleting a StatefulSet deletes the associated pods in an orderly fashion, preserving their unique identities. | Deleting a Deployment deletes all the associated pods simultaneously. |
| Supports preserving the identity of volumes when pods are rescheduled. | Does not preserve the identity of volumes when pods are rescheduled. |

#### **Creating StorageClass and PVC**

First, let's create a StorageClass and a PersistentVolumeClaim (PVC) for our StatefulSet:

### StorageClass for AWS
```
apiVersion: storage.k8s.io/v1
kind: StorageClass
metadata:
  name: ebs-sc
provisioner: ebs.csi.aws.com
parameters:
  type: gp2
volumeBindingMode: WaitForFirstConsumer
```

### Storage Class for local Setup
```
apiVersion: storage.k8s.io/v1
kind: StorageClass
metadata:
  name: local-storage
provisioner: rancher.io/local-path
volumeBindingMode: WaitForFirstConsumer
allowVolumeExpansion: true
reclaimPolicy: Retain
```

#### **Creating StatefulSet with MySQL**

Now, let's create a StatefulSet with a MySQL database using the created StorageClass and PVC:

```
apiVersion: apps/v1
kind: StatefulSet
metadata:
  name: mysql-statefulset
spec:
  selector:
    matchLabels:
      app: mysql
  serviceName: mysql-service
  replicas: 3
  template:
    metadata:
      labels:
        app: mysql
    spec:
      containers:
        - name: mysql
          image: mysql:5.7
          ports:
            - containerPort: 3306
          env:
            - name: MYSQL_ROOT_PASSWORD
              valueFrom:
                secretKeyRef:
                  name: mysql-secret
                  key: root-password
          volumeMounts:
            - name: mysql-volume
              mountPath: /var/lib/mysql
  volumeClaimTemplates:
    - metadata:
        name: mysql-volume
      spec:
        accessModes:
          - ReadWriteOnce
        storageClassName: aws-storage
        resources:
          requests:
            storage: 10Gi
```

#### **Important StatefulSet Commands**

- Create or apply a StatefulSet:

  ```
  kubectl apply -f statefulset.yaml
  ```

- Get StatefulSets:

  ```
  kubectl get statefulsets
  ```

- Get StatefulSet pods:

  ```
  kubectl get pods -l app=mysql
  ```

- Scale a StatefulSet:

  ```
  kubectl scale statefulset mysql-statefulset --replicas=5
  ```

- Delete a StatefulSet and associated pods:

  ```
  kubectl delete statefulset mysql-statefulset
  ```

These commands provide basic functionality for managing a StatefulSet in Kubernetes. You can use them to create, scale, and delete StatefulSets according to your application requirements

.

Remember to adjust the YAML configurations, such as image, storage size, and passwords, according to your specific needs before applying the manifests.