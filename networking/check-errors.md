# Check for errors on the network interfaces

```bash
$ ip -s link
1: lo: <LOOPBACK,UP,LOWER_UP> mtu 65536 qdisc noqueue state UNKNOWN mode DEFAULT group default qlen 1000
    link/loopback 00:00:00:00:00:00 brd 00:00:00:00:00:00
    RX: bytes  packets  errors  dropped overrun mcast   
    442771     4914     0       0       0       0       
    TX: bytes  packets  errors  dropped carrier collsns 
    442771     4914     0       0       0       0       
2: enp24s0: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1500 qdisc fq_codel state UP mode DEFAULT group default qlen 1000
    link/ether 30:9c:23:de:7a:b9 brd ff:ff:ff:ff:ff:ff
    RX: bytes  packets  errors  dropped overrun mcast   
    2361031484 1747353  0       8486    0       123     
    TX: bytes  packets  errors  dropped carrier collsns 
    91521720   917118   0       0       0       0       
```