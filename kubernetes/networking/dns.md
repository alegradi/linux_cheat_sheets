# DNS in Kubernetes

Reach address within the cluster by using:
- http://web-service
- http://web-service.apps (namespace is apps)
- http://web-service.apps.svc
- http://web-service.apps.svc.cluster.local
- ip can be converted to dashed hostname: 10.244.1.5 - 10-244-1-5.apps.pod.cluster.local (reaching a pod)

The easiest to troubleshoot reachability issues with logging into the pod that needs to reach another one and using `curl` from there.

## CoreDNS in Kubernetes

CoreDNS config file
```bash
/etc/coredns/Corefile
```

The root domain/zone configured for the cluster can be seen in the coredns configmap:
```bash
kubectl get configmap coredns -n kube-system -o yaml
...
        }
        ready
        kubernetes cluster.local in-addr.arpa ip6.arpa {
```

