# Application Lifecycle Management

kubectl rollout status deployment/myapp-deployment  (to check status of an upgrade)
kubectl rollout undo deployment/myapp-deployment  (to undo an upgrade and roll back to previous state)

## Commands and args
Anything you would specify as an argument in `docker run` should go to:  
```
spec:
  containers:
    args: ["10"]  (for example, overwrites CMD)
```

If you want to specify an entrypoint it should be like this:  
```
spec:
  containers:
    command: ["python3.6"]  (overwrites ENTRYPOINT)
    args: ["pip", "list"]  (for example, overwrites CMD)
```

## Environment variables
Pod definition should be:
```
spec:
  containers:
    env:
      - name: APP_COLOR
        value: pin
```
This is the same as `docker run -e APP_COLOR=pink "container_name"`  
It can also be:
```
spec:
  containers:
    env:
      - name: APP_COLOR
        valueFrom:
          configMapKeyRef:  or
          secretKeyRef:
``` 

## Configmap
kubectl get configmaps  (to see configmaps)

### Imperative way
kubectl create configmap  (imperative way)  

kubectl create configmap "config_name" --from-literal="key"="value"  
kubectl create configmap app-config --from-literal=APP_COLOR=blue  (for example, for further values use --from-literal option again in the same command)  
kubectl create configmap  "config_name" --from-file="path/to/file"  (to use a file to create a configmap)  

### Declarative way
kubectl create -f  (declarative way)  
```
apiVersion: v1
kind: ConfigMap
metadata:
  name: app-config
data:
  APP_COLOR: blue
  APP_MODE: prod
```
kubectl create -f config-map.yaml

Example configmap for mysql
```
port: 3306
max_allowed_packet: 128M
```

To use configmap in a pod definition use:
```
spec:
  containers:
    envFrom:
      - configMapRef:
          name: app-config  (name a of the configmap)
```
