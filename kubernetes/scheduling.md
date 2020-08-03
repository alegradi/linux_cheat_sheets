# Kubernetes - Scheduling

## Manual scheduling
- Can be done at creation time with editing the definition.yml
```
spec:
  nodeName: "nodename"
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

## Labels and selectors
kubectl get pods --selector "label=value"  (to filter by matches)
kubectl get pods --selector env=prod,tier=frontend,bu=finance  (for example, applying several filters)
kubectl get pods -l "label=value"  (same as --selector)

kubectl label nodes "node_name" "key"="value"  (to add a label to a node, eg. size=Large)  
Pod definition to match label should be like:
```
spec:
  nodeSelector:
    size: Large
```

## Taints and toleration
kubectl describe node kubemaster | grep Taint  (to verify taints on the master node)  
kubectl taint nodes "node_name" "key"  (to taint a node)  
kubectl taint nodes node-name key=value:taint-effect  (example for the above)(taint-effect can be NoSchedule, PreferNoSchedule, NoExecute)
- NoSchedule = Don't schedule pod here that is not allowed
- PreferNoSchedule = Preferably don't schedule pod here that is not allowed
- NoExecute = Existing non conforming pods are evicted 

Toleration setting, example
```
spec:
  tolerations:
  - key: "app"
    operator: "Equal"
    value: "blue"
    effect: "NoSchedule"
```

## Affinity
Example:  
```
spec:
  affinity:
    nodeAffinity:
      requiredDuringSchedulingIgnoreDuringExecution:  (or preferredDuringSchedulingIgnoreDuringExecution)
        nodeSelectorTerms:
        - matchExpressions:
          - key: size  (example)
            operator: In  (or NotIn, or Exists, in which case you don't specify values)
            values:
            - Large  (example)
```

## Resource requirement and limits
Example of resource requirement in pod-definition for each container
```
spec:
  resources:
    requests:
      memory: "1Gi"
      cpu: 1
```  

cpu unit 1 = 1 vCPU

first header   | second header
---------------|---------------
1 G (Gigabyte) | 1,000,000,000 bytes
1 M (Megabyte) | 1,000,000 bytes
1 K (Kilobyte) | 1,000 bytes
1 Gi (Gibibyte)| 1,073,741,824 bytes
1 Mi (Mebibyte)| 1,048,576 bytes
1 Ki (Kibibyte)| 1,024 bytes

Setting limits example:
```
spec:
  resources:
    limits:
      memory: "2Gi"
      cpu: 2
```  




