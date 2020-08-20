# Cluster Maintenance

kubectl drain "node"  (to drain pods from it)  
kubectl uncordon "node"  (to remove cordon from a node)  
kubectl cordon "node"  (marks node unschedulable, no new pods will be scheduled on it)  

## Upgrades
kubeadm upgrade plan  (to show the possible upgrade plan)  
kubeadm upgrade apply "version"  (to apply an upgrade plan for the specified version)  

### Upgrading the master
- apt-get upgrade kubeadm="version"
- apt-get upgrade kubelet="version"
- sytemctl restart kubelet.service

### Upgrading the worker(s)
- kubectl drain "node"
- apt-get upgrade kubeadm="version"
- apt-get upgrade kubelet="version"
- kubeadm upgrade node config --kubelet-version v"version"
- sytemctl restart kubelet.service
- kubectl uncordon "node"