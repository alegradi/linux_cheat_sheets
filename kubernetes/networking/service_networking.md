# Service networking

Services (ClusterIP) are accessible form all pods on the cluster. 

Services are only virtual objects, being accessed by forwarding rules created by `kube-proxy`. It uses `iptables` by default.

A service's IP is picked from the range defined by
```
kube-api-server --service-cluster-ip-range ipNet (Default 10.0.0.0/24)
```

# NodePort
Gets an internal IP that any Pod can use to communicate with it. It also creates a high-number port for external access.