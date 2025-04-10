---
tags:
  - k3s
  - storage
---
A persistent volume is a piece of storage in a [[Kubernetes]] cluster that is independently managed and can be used by Pods to persist data.

It allows for the decoupling of storage management from the lifecycle of individual pods. This means that even if a pod is deleted the storage remains and can be reaccessed if the pod has to be restarted.

## Why use them?
- Data Persistence - This is useful if your pods rely on databases as the database doesn't get deleted when the pod does.
- Decoupling Storage from Pods - The storage can be managed separately from the pods meaning changes can be made whilst the pod is running.
- Storage Abstraction - Means that storage can be handled by different backend implementations such as local path, NFS, or cloud solutions.

## Key Concepts
1. Make a Persistent Volume (PV)
	- Create a cluster wide resource for storage
2. Persistent Volume Claim (PVC)
	- A request for storage by an application
	- Specifies the size, access modes and storage class to use.
	- PVC is bound to a matching PV by Kubernetes and once bound the data is accessible by a Pod
3. Storage Class
	- Defines different types of storage in Kubernetes, such as SSD, NFS or Cloud Storage
	- Storage Class defines the provisioner, parameters and reclaim policy for the storage
	- PVC can request a particular storage class to use.
4. Reclaim Policy
	- Defines what happens to a PV when the PVC is deleted
	- Retain: Keeps the PV until manually handled.
	- Recycle: Deletes the PVs contents and prepares it for reuse.
	- Delete: Deletes the PV entirely (this is how cloud providers handle it.)

## Example PV
```yaml
apiVersion: v1
kind: PersistentVolume
metadata:
	name: my-pv
spec:
	capacity:
		storage: 5Gi
	volumeMode: Filesystem
	accessModes:
		- ReadWriteOnce
	persistentVolumeReclaimPolicy: Retain
	storageClassName: localpath
	hostPath:
		path: "/mnt/data"
```

In this example it has allocated 5Gb in size. The accessMode means that the PersistentVolume can only be mounted by a single node.
hostPath represents where the volume will be mounted inside the host.

## Example PVC
```yaml
apiVersion: v1
kind: PersistentVolumeClaim
metadata:
	name: my-pvc
spec:
	accessModes:
		- ReadWriteOnce
	resources:
		requests:
			storage: 5Gi
	storageClassName: localpath
```

## Using with a pod
```yaml
apiVersion: v1
kind: Pod
metadata:
	name: my-app-pod
spec:
	containers:
		- name: my-app-container
		  image: my-app-image
		  volumeMounts:
			- mountPath: "/data"
			  name: my-storage
	volumes:
		- name: my-storage
		  persistentVolumeClaim:
			  claimName: my-pvc
```

This mounts the data inside the pod at the /data path. And the claim name must match that of the PVC that is being used.

## Dynamic Provisioning
In many cloud environments there is no need to manually create PVs. They can be automatically created by the storage class being used for the PVC.