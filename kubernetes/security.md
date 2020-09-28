# Kubernetes - Security

## kube-apiserver - basic auth mechanism (Not recommended)
1, Pass in a text file with password,username,userid
pod definition needs to have the file referenced like this:
`--basic-auth-file=user-details.csv`
Also update the '-mountPath: and -hostPath: path:' setting to mount the file

Create the required role and role bindings for the users:
```
    ---
    kind: Role
    apiVersion: rbac.authorization.k8s.io/v1
    metadata:
      namespace: default
      name: pod-reader
    rules:
    - apiGroups: [""] # "" indicates the core API group
      resources: ["pods"]
      verbs: ["get", "watch", "list"]
     
    ---
    # This role binding allows "jane" to read pods in the "default" namespace.
    kind: RoleBinding
    apiVersion: rbac.authorization.k8s.io/v1
    metadata:
      name: read-pods
      namespace: default
    subjects:
    - kind: User
      name: user1 # Name is case sensitive
      apiGroup: rbac.authorization.k8s.io
    roleRef:
      kind: Role #this must be Role or ClusterRole
      name: pod-reader # this must match the name of the Role or ClusterRole you wish to bind to
      apiGroup: rbac.authorization.k8s.io
```

This can be then utilised by 
`curl -v -k https://master-node-ip:6443/api/v1/pods -u "user1:password123"`

2, Pass in static token file
`--token-auth-file=user-details.csv`

Use it then with:
`curl -v -k https://master-node-ip:6443/api/v1/pods --header "Authorization: Bearer dfiodlkjfpsadojfasdjfpjaisd"`

kubectl create serviceaccount

## SSL verification
openssl x509 -in /etc/kubernetes/pki/apiserver.crt -text -noout  (to view details of the certificate)
openssl req -in /path/to.csr -text -noout -verify  (to verify a csr)

## Certificate signing
kubectl get csr  (to show pending certificate signing requests)  
kubectl certificate approve "name"  (to approve a signing request)

```
apiVersion: certificates.k8s.io/v1beta1
kind: certificateSigningRequest
metadata:
  name: name
spec:
  groups:
  - system:authenticated
  request: LS0tLS1CRUdJTiBDRVJUSUZJQ0FURSBSRVFVRVNULS0tLS0KTUlJQ1ZqQ0NBVDRDQVFBd0VURVBNQTBHQTFVRUF3d0dZV3R6YUdGNU1JSUJJakFOQmdrcWhraUc5dzBCQVFFRgpBQU9DQVE4QU1JSUJDZ0tDQVFFQXBjUWo5M2JKcnZsdStYTXkrNlAyRndoakV0OEVkVXlBek5kRndLSUpYc3Z4CnNDUjIrS2FqTlNvY2dLTFozaDNYemFmOGM5cEJpZU1TWnBXSlZCbGxOdS9oeEF1aWtORU1SeGdjNWJXUGFOc0IKbEZoNUF1TmVrdjVGNkN6eDhaT0ZXRlpXdXcwS3JEbGd2K09hcldCU0p0U0ZtcWdEUlZVVXdMSGlHTHpFb2NRWApFUWxpUGVRSE5LeE5QZWZ4R1FSVzIwZkVoN1VoUVQvVkpyTHZ3Qk8ySHR2b01BNWhwK1U1VzZ2bzlobkMxYXpSClF3STV0SERQcVd6R0JZT1FjcTFIcDk5RExYNHpsaXhmMGNLdUtLOEkwRGoyUDhHM1d6K3FKQ1Y5V0J6NE8wRGUKbGRIMld2NVBtTURVQ0N5VEM0ODFCd3BaNHNVcUFlV09QZG41WkVUemZ3SURBUUFCb0FBd0RRWUpLb1pJaHZjTgpBUUVMQlFBRGdnRUJBSC9WZmRLWGVaT0JxWEdBajZJemJtZ3VhaThwZGhCcTJsUEZRcWQyem5BWjVJaFVqekdIClRSbTM2YmtOTm1TSjUzRi82cGVIaHV4SXN4VUJORWRXNHBaOXQwbkVtQXRBYVdRbFI3OG1IV0I0c0RJNEV6dVAKMUV4V1pmWVJwWk9XQ2dzZFI4aGpaWngxQ3p1UUtjSnVnZjRRSzIwQXpFSERFNUdBVFFvejlKWXVrZ0hPb29NNQpuT1NVL2NvbE52MklONm8xZnU2cTJXOXFWTzUvMi8wYVlyOTlPbHVLTmFRU1QxY0dJL0owbDV1RG9pZ0pxdHZ5Ck9oeG45enZLbzluYzVmVkFhaFFZK08vMWZFaDE5Z1JSaTVpL0FxSnV1T2c0L0dRVFd3bVN5VmpvOTFoV1dSNSsKbWV4bXNBcmw3NGhPck1VM3pBTUcwMks3d2lIamRjNms4UVE9Ci0tLS0tRU5EIENFUlRJRklDQVRFIFJFUVVFU1QtLS0tLQo=  (csr base64 encoded)
  usages:
  - digital signature
  - key encipherment
  - server auth
```

## Kube config

kubectl config -h  (to see list of options)  
kubectl config view  
kubectl config view --kubeconfig=my-custom-config  (to specify custom config to use)  
kubectl config use-context prod-user@production  (to change the context on the fly)  

## API access

kubectl proxy  (to start a proxy with the certificates used, no need to specify them in curl)
  curl http://localhost:8001 -k  (to see API options and connect to the proxy)

## RBAC

kubectl get roles  (to see active roles)  
kubectl get rolebindings  (to see active role bindings)  
kubectl auth can-i create deployment  (to see if you have permission to do something)  
kubectl auth can-i delete nodes  
kubectl auth can-i delete nodes --as user  (to check if a certain user has access to something, you need to be admin for this)  

developer-role.yml
```
apiVersion: rbac.authorization.k8s.io/v1
kind: Role
metadata:
  name: developer
rules:
- apiGroups: [""]
  resources: ["pods"]
  verbs: ["list", "get", "create", "update", "delete"]  (what they are allowed to do)

- apiGroups: [""]
  resources: ["ConfigMap"]
  verbs: ["create"]
```

kubectl create -f developer-role.yml

devuser-developer-binding.yml
```
apiVersion: rbac.authorization.k8s.io/v1
kind: RoleBinding
metadata:
  name: devuser-developer-binding
subjects:
- kind: User
  name: dev-user
  apiGroup: rbac.authorization.k8s.io
roleRef:
  kind: Role
  name: developer
  apiGroup: rbac.authorization.k8s.io
```

kubectl create -f devuser-developer-binding.yml