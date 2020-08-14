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