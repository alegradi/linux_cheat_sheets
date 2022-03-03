# JSONPath - useful examples

Get SSL certificate from K8s
```bash
kubectl get secret -n app app-tls -o jsonpath='{.data.tls\.crt}' | base64 --decode
```

Show pods and sort by host nodename
```bash
k get pods -n kube-system -o wide --sort-by='{.spec.nodeName}'
```