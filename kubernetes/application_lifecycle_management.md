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
