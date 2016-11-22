[server.conf](etc/openvpn/server.conf)

This file contains configurations for openvpn server side

```bash
# server mode and use TLS for extra security
mode server
tls-server
# uses port 1194 over UDP and not TCP.
port 1194
proto udp 
# tun enables the host to use a tunnel interface
dev tun
# This configuration is mapping an IP to the client on subnet 10.8.0.0/24
server 10.8.0.0 255.255.255.0

# This configuration may need to be updated in the future if more subnets are added.
# This is telling the client to route these subnets through the VPN server.
push "route 10.0.0.0 255.255.255.0"
push "route 10.0.1.0 255.255.255.0"
push "route 10.0.2.0 255.255.255.0"
push "route 10.0.3.0 255.255.255.0"

# DNS options for querying.  10.0.0.2 is the internal DNS server Amazon creates.
# This allows the AWS service endpoints to use the private IP address. 
# The next one is OpenDNS which allows to query public records if the record 
# does not exist in 10.0.0.2
push "dhcp-option DNS 10.0.0.2" 
push "dhcp-option DNS 208.67.220.220"

# This line is important as if you revoke certificates, users will still 
# be able to connect unless this line is present.
crl-verify crl.pem

# This option I disabled from the default template.  I only want OpenVPN to route 
# traffic to AWS VPC.  If you query public records, that should just go over the 
# regular internet and not through the VPN.  Also, the server does not need to 
# do any POSTROUTING in iptables.

#push "redirect-gateway def1 bypass-dhcp"
```

[sysctl.conf](etc/sysctl.conf)

Only setting that needed to be changed is to enable ipv4 forwarding.  
