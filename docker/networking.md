# Docker networking 

Check current setting with:  
```bash
docker network ls
```

Types:  
- `none`  - No access to the container from the outside world, or vice versa  
- `host`  - Container shares the network of the host, for example container listening on port 80 will be available on port 80 on the host. This can cause conflict if you want another instance of the same  
- `bridge`  - Docker creates a virtual private network with a bridge device. default 172.17.0.1/24  

## Bridge network

Default docker network is created as `bridge` and called also the same.
```bash
docker network ls
NETWORK ID     NAME      DRIVER    SCOPE
899f111d9c34   bridge    bridge    local
1923a48f2c5c   host      host      local
c97fc60ab572   none      null      local
```

On the host however this is called `docker0` and with a default address
```bash
ip addr | grep docker0: -A3
3: docker0: <NO-CARRIER,BROADCAST,MULTICAST,UP> mtu 1500 qdisc noqueue state DOWN group default 
    link/ether 02:42:2f:78:58:00 brd ff:ff:ff:ff:ff:ff
    inet 172.17.0.1/16 brd 172.17.255.255 scope global docker0
       valid_lft forever preferred_lft forever
```

### Bridge network details

The bridge network is created like this:
```bash
ip link add docker0 type bridge
```

Docker also creates a network namespace for it. Check it for a running container:
```bash
docker inspect 8b6c10525b18 | grep -i SandboxKey
            "SandboxKey": "/var/run/docker/netns/2c6a944cba0f",
```
Every time there is a container created, docker does:
- Create a networking namespace for it
- Attach the interface from the container to it
- Attach the interface on the host to it. Odd and even numbers for the interfaces form a pair (ex: if9 - if10)

## Port mapping
From the host you can always reach the port on the container's address:
```bash
curl http://172.17.0.2:80 -IL
HTTP/1.1 200 OK
Server: nginx/1.21.4
```
To make it publicly available on the host's network, run the container with mapping the port
```bash
docker run -p 8080:80 nginx
```

This is done with `iptables` NAT rules:
```bash
docker run --rm -d -p 8080:80 nginx

iptables -t nat -vnL
...
   0     0 DNAT       tcp  --  !docker0 *       0.0.0.0/0            0.0.0.0/0            tcp dpt:8080 to:172.17.0.2:80
```
The rule is:
```bash
iptables -t nat -A DOCKER -j DNAT --dport 8080 -to-destination 172.17.0.2:80
```

## CNI - Container networking interface

