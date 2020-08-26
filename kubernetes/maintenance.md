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

## Backup and restore
kubectl get all --all-namespaces -o yaml > all-deploy-services.yaml  (to backup all resources by the API querried)

To use the etcd snapshot utility
- export ETCDCTL_API=3
- etcdctl snapshot save snapshot.db for examle:  
`etcdctl snapshot save /tmp/snapshot-pre-boot.db --cacert=/etc/kubernetes/pki/etcd/ca.crt --cert=/etc/kubernetes/pki/etcd/server.crt --endpoints=https://127.0.0.1:2379 --key=/etc/kubernetes/pki/etcd/server.key`
- a good way of testing the above is correct, replace the `snapshot save` with `etcdctl member list`
- etcdctl snapshot status snapshot.db  (to view the status of the snapshot) 


### Restoring from an etcd snapshot
- service kube-apiserver stop  (might not be needed)
- export ETCDCTL_API=3 
- etcdctl snapshot restore snapshot.db --data-dir /var/lib/etcd-from-backup --initial-cluster master-1=https://192.168.5.11:2380,master-2=https://192.168.5.12:2380 \
--initial-cluster-token etcd-cluster-1 --initial-advertise-peer-urls https://${INTERNAL_IP}:2380
- update the etcd static pod definition file to match the above settings, specifically the data-dir and cluster-token
- restart etcd  (should do it automatically)
- start kube-apiserver  (might not be needed)
