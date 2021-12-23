# Cluster networking

## Port requirements
Master:  
- 2379 (etcd)  
- 2380 (etcd)  
- 6443 (api-server)  
- 10250 (kubelet)  
- 10251 (kube-scheduler)  
- 10252 (kube-controller-manager)  

Workers:  
- 2380 (etcd)  
- 10250 (kubelet)  
- 30000 - 32767 (general access)  

## Networking solution
Checking what networking solution is in place:
```bash
ls /etc/cni/net.d/
```
