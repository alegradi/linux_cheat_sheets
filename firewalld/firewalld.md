# Firewalld
## Basics
firewall-cmd --reload  (to reload firewall)  

firewall-cmd --list-all  (to get list of current rules and settings)  
firewall-cmd --get-services  (to list all known services that can be added)  
firewall-cmd --add-service=http  (add rule runtime)  
firewall-cmd --add-service=http --permanent  (adds to firewall config, but not current config, reload needed)  
firewall-cmd --add-port=70/tcp  (to add a port to current zone)  
firewall-cmd --add-port={70/tcp,5000/tcp}  (to add multiple ports to current zone)  
firewall-cmd --add-port=70-75/tcp  (to add a range of ports to current zone)  


firewall-cmd --remove-port=70/tcp  (to remove port)  
firewall-cmd --remove-service=ssh  (to remove service)  

firewall-cmd --get-zones  (to get available zones)  
firewall-cmd --list-all --zone=work  (to show rules for the specified zone)    

## Port forwarding and traffic redirection  (not verified to be working)  
This needs sysctl net.ipv4.ip_forward set to 1  
`sysctl -w net.ipv4.ip_forward=1` or `echo 1 > /proc/sys/net/ipv4/ip_forward`  

firewall-cmd --add-forward-port=port=15000:proto=tcp:toport=10050  (local port forwarding from 15000 to 10050)  
firewall-cmd --add-forward-port=port=15000:proto=tcp:toport=10050:toaddr=192.168.0.122  (for remote forwarding)  

## Rich rules
firewall-cmd --add-rich-rule='rule family="ipv4" source address="192.168.0.17" accept'  (to accept all from the source address)  
firewall-cmd --add-rich-rule='rule family="ipv4" source address="10.1.0.55" drop'  (to drop all from the source address)  
