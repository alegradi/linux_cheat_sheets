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
