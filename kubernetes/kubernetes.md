# Kubernetes

## Basics

kubectl get nodes  (to see nodes in the cluster)  
kubectl get pods  (to see active pods)  
kubectl -n kube-system get pods  (to see kubernetes's own pods, these are the system components)  
kubectl create -f "pod_definition.yml"  (to create a pod from definition)  