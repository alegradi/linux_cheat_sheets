# Kubernetes - Metrics

## Basics
kubectl top pod  (to check resource usage if metrics is installed and runnign)  
kubectl top node  (to check load on nodes)  

## Logging
kubectl logs "podname"  (to check logs of a pod)  
kubectl logs -f "podname" "container_name"  (to follow logs of a container from a pod with several containers)  
