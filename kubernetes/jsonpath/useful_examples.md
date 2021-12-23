# JSONPath - useful examples

Get SSL certificate from K8s
```bash
kubectl get secret -n app app-tls -o jsonpath='{.data.tls\.crt}' | base64 --decode
```

