# Iptables - Permanent rules (old method)

This method can be used on old systems (Debian 4).

1) Add rules manually to iptables
```bash
iptables -A INPUT -p tcp -m tcp --dport 443 -j ACCEPT
```

2) Write rules to a file
```bash
iptables-save > /etc/iptables.rules
```

3) Set up script to load rules on boot
- /etc/network/if-pre-up.d/firewall (Debian 4)
```bash
#!/bin/bash
/sbin/iptables-restore < /etc/iptables.rules
```

4) Make it executable