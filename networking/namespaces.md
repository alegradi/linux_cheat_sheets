# Linux Network Namespaces

## Basics
ip netns  (to list existing namespaces)  
ip netns add "namespace  (to create the namespace)   

ip netns exec "namespace" ip link (to see interfaces within that namespace)  
ip -n "namespace" link  (to see available interfaces in that namespace)  

ip link add veth-"namespace1" type veth peer name veth-"namespace2"  (to create link between two namespaces)  
ip -n "namespace" link del veth-"namespace"  (to delete a link, other end gets deleted automatically)  
ip link set veth-"namespace" netns "namespace"  (to attach the interface to the namespace)  
ip -n "namespace1" addr add 192.168.12.1/24 dev veth-"namespace1"  (to add an IP to the interface) (always include netmask!!) 
ip -n "namespace1" link set veth-"namespace1" up  (to bring up the interface)  

## Virtual switch
Using examples of `red` and `blue` namespaces
ip link add v-net-0 type bridge  (to create the virtual network called v-net-0)  
ip link set dev "devname" up  (to bring up the interface)  

Attach virtual cable between a device and the virtual switch
`ip link add veth-red type veth peer name veth-red-br`
`ip link add veth-blue type veth peer name veth-blue-br`  (and for blue)  

Assign the interface with the namespace
`ip link set veth-red netns red`
`ip link set veth-blue netns blue`  (and for blue)

Attach the device in the namespace to the virtual switch
`ip link set veth-red-br master v-net-0`
`ip link set veth-blue-br master v-net-0`  (and for blue)

Add IP addrasses to the links
`ip -n red addr add 192.168.15.1/24 dev veth-red`
`ip -n blue addr add 192.168.15.2/24 dev veth-blue`  (and for blue)

Bring the interfaces up
`ip -n red link set veth-red up`
`ip -n blue link set veth-blue up`  (for blue)

### Note
To map a port on the host to port on a container (for instance) you can use iptables with
```
iptables \
    - t nat \
    - A PREROUTING \
    - j DNAT \
    --dport 8080 \
    --to-destination 172.17.0.3:80  (ip can be omitted if natting locally only)
```

Postrouting
```
iptables \
    - t nat \
    - A POSTROUTING \
    - s 192.168.15.0/24 \
    - j MASQUERADE
```