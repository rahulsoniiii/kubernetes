### Access Modes and PersistentVolumeReclaimPolicy in Kubernetes

## Access Mode:
- Access Mode in Kubernetes specifies how a Persistent Volume (PV) can be accessed by Pods. There are three access modes:

1. ReadWriteOnce (RWO): Allows the volume to be mounted as read-write by a single Node.

2. ReadOnlyMany (ROX): Allows the volume to be mounted as read-only by multiple Nodes.

3. ReadWriteMany (RWX): Allows the volume to be mounted as read-write by multiple Nodes.

## PersistentVolumeReclaimPolicy:
- PersistentVolumeReclaimPolicy determines what happens to the Persistent Volume (PV) when the associated Persistent Volume Claim (PVC) is deleted. There are three reclaim policies:

1. Retain: Retains the PV and its data even after the PVC is deleted. Manual cleanup of the PV is required.

2. Delete: Deletes the PV and its data automatically when the PVC is deleted.

3. Recycle (Deprecated): Performs a basic cleanup by deleting the contents of the PV when the PVC is deleted. This policy is deprecated and no longer recommended.