# CNI - Container networking interface

A set of standards a network plugin provider needs to follow.

CNI is configured on the `Kubelet`. Always check the config for CNI info
```bash
ps axfu | grep kubelet
```

CNI executables are at:
```bash
/opt/cni/bin
```

Verify the available networks for pods by checking the config in:
```bash
cat /etc/cni/net.d/*.conf

...
      "ipam": {
        "type": "calico-ipam",
        "assign_ipv6": "true",
        "ipv6_pools": ["fd52:5b5b:b0ab:f430:0:0:1::/116"],
        "assign_ipv4": "true",
        "ipv4_pools": ["10.233.64.0/18"]
```

Subnetting info
```
10.233.64.0/18
10.233.64.1 - 10.233.127.254

10.233.0.0/18
10.233.0.1 - 10.233.63.254
```