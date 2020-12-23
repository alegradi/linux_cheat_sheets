# Kubernetes network

Checking what networking solution is in place:
`cat /etc/cni/net.d/*.config`

## Pod networking
`ps axfu | grep -i kubelet | grep -i cni`  to see where the config is

## weaveworks (CNI)

To verify ranges used by pods/services etc the best option is to check the logs of the networking pods
kubectl logs -n kube-system "podname" 

## CoreDNS
Find how the DNS server is exposed to all pods:
`kubectl get service -n kube-system`

Find the relevant config for the DNS server with:
`kubectl describe configmap coredns -n kube-system`

## Ingress
Layer 7 load balancer built into the cluster. 
Ingress controller probably Nginx

To see deployed ingresses:  
`kubectl get ingress`  

Deployment file for ingress controller
```
apiVersion: apps/v1
kind: Deployment
metadata:
  name: nginx-ingress-controller
spec:
  replicas: 1
  selector:
    mathcLables:
      name: nginx-ingress
  template:
    metadata:
      labels:
        name: nginx-ingress
    spec:
      containers:
        - name: nginx-ingress-controller
          image: quay.io/kubernetes-ingress-controller/nginx-ingress-controller:0.21.0
      args:
        - /nginx-ingress-controller
        - --configmap=$(POD_NAMESPACE)/nginx-configuration  ## Makes sense this way to easily configure nginx
      env:
        - name: POD_NAME
          valueFrom:
            fieldRef:
              fieldPath: metadata.name
        - name: POD_NAMESPACE
          valueFrom:
            fieldRef:
              fieldPath: metadata.namespace
      ports:
        - name: http
          containerPort: 80
        - name: https
          containerPort: 443
```

Then create a service to expose the ingress controller
```
apiVersion: v1
kind: Service
metadata:
  name: nginx-ingress
spec:
  type: NodePort
  ports:
  - port: 80
    targetPort: 80
    protocol: TCP
    name: http
  - port: 443
    targetPort: 443
    protocol: TCP
    name: https
  selector:
    name: nginx-ingress
```

Service Account
```
apiVersion: v1
kind: ServiceAccount
metadata:
  name: nginx-ingress-serviceaccount
```