# Kubernetes - Scheduling

## Manual scheduling
- Can be done at creation time with editing the definition.yml
```
spec:
  nodename: "nodename"
```
- Or need to mimic the bind-definition.yml by sending a post request
```
apiVersion: v1
kind: Binding
metadata:
  name: nginx
target:
  apiVersion: v1
  kind: Node
  name: node02  (this is the target)
```
Becomes:  
` curl --header "Content-Type:application/json" --request POST --data '{"apiVersion":"v1", "kind":"Binding", ...} http://$SERVER/api/v1/namespaces/default/pods/$PODNAME/binding/ `
