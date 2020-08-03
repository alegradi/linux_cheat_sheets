# Kubernetes - WIP

This is still work in progress and badly needs organising

## 
kubectl run --generator=run-pod/v1 nginx --image=nginx  (to create an nginx pod)
kubectl run --generator=run-pod/v1 nginx --image=nginx --dry-run -o yaml  (this will print the basic yaml of the config)  



## Basics
kubectl get nodes  (to see nodes in the cluster)  
kubectl get pods  (to see active pods) 
kubectl get pods -o wide  (to get more info)  
kubectl get pod "podname" -o yaml > file.yml  (to create a pod definition yaml out of an existing pod)   
kubectl run "name" --image="image_name"  (to create a pod)  
kubectl -n kube-system get pods  (to see kubernetes's own pods, these are the system components)  
kubectl get pods --namespace=kube-system  (same as above)  
kubectl create -f "pod_definition.yml"  (to create a pod from definition)  
kubectl apply -f "filename"  (to apply changes)
... --dry-run=client  (to verify if config is correct)

### Replicasets
kubectl get replicaset  (to get replicasets)  
kubectl create -f "replicaset.yml"  (to create replicaset)  
kubectl replace -f "replicaset-definition.yml"  (to read in new values)  

kubectl scale --replicas="num" deployment "deployment_name"  
kubectl scale --replicas=3 replicaset/new-replica-set  

kubectl delete replicaset "replicaset-name"  (to remove replicaset and all pods within) 

### Deployments
kubectl get deployments  (to see deployments)  
kubectl get all  (to see all resources deployed, eg. deployment, replicaset... etc)  

kubectl create deployment --image=nginx nginx  (to create a deployment of nginx)
kubectl create deployment --image=nginx nginx --dry-run -o yaml  (to write the yaml config to stdout)  

kubectl expose deployment "name" --type=NodePort --target-port=8080 --name=webapp-service --dry-run o yaml > service.yml

### Namespace
kubectl get ns  (to get namespaces, you can also put "get namespace")  

... --namespace="namespace"  (to do any of the above in a different namespace)  
... --all-namespaces  (to do it in all namespaces, eg. "get pods")
... --no-headers  (to not have headers, good for | wc -l)  

kubeclt create namespace "namespace"  (to create a namespace)  
kubectl config set-context $(kubectl config current-context) --namespace="namespace"  (to switch over to another namespace. You don't need to specify it anymore, working on it)  

### Services
kubectl get svc  (to get services, same as "get service")  
kubectl expose deployment "name" --type=NodePort --target-port=8080 --name=webapp-service --dry-run o yaml > service.yml  (to create the yaml)  
kubectl expose pod redis --port=6379 --name redis-service --dry-run=client -o yaml  (to create a redis service config, listening on port 6379)  
kubectl expose pod nginx --port=80 --name nginx-service --type=NodePort --dry-run=client -o yaml  (to create a service, exposing nginx's port 80, this allows the selector but not specifying the node port)  
kubectl create service nodeport nginx --tcp=80:80 --node-port=30080 --dry-run=client -o yaml  (this allows you to specify the nodeport, but cannot use selector)  
kubectl get ep "service_name"  (to get endpoint list)  

kubectl expose pod redis --port=6379 --name redis-service --dry-run=client -o yaml > redis-service.yml  (to create a service based on the pod)  
