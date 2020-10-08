# Kubernetes - Storage

## Volumes
Definition for local volume
```
...
spec:
  containers:
    ...
    volumeMounts:
    - mountPath: /opt
      name: data-volume
  ...
  volumes:
  - name: data-volume
    hostPath:    ## This mounts it from a local storage, avoid it in multinode clusters
      path: /data
      type: Directory  
```

Definition for AWS storage
```
...
spec:
  containers:
  ...
  volumes:
  - name: data-volume
    awsElasticBlockStore:
      volumeID: <volume-id>
      fsType: ext4  
```

## Persistent volumes
Definition:
```
apiVersion: v1
kind: PersistentVolume
metadata:
  name: pv-vol1
spec:
  accessModes:
    - ReadWriteOnce
  capacity:
    storage: 1Gi
  hostPath:    ## Only local storage, not to be used on a cluster
    path: /tmp/data
```

To allow a persistent volume claim to directly use this volume:
```
labels:
  name: my-pv
```

## Persisten Volume Claims
Definition:
```
apiVersion: v1
kind: PersistentVolumeClaim
metadata:
  name: myclaim
spec:
  accessModes:
    - ReadWriteOnce
  resources:
    requests:
      storage: 500Mi
```

For assigning to a specific volume, you can use:
```
spec:
  selector:
    matchLabels:
      name: my-pv
```

Define a claim in a pod like this:
```
apiVersion: v1
kind: Pod
metadata:
  name: mypod
spec:
  containers:
  containers:
    ...
  volumes:
    - name: mypd
      persistentVolumeClaim:
        claimName: myclaim
```


