## main.yml
The playbook will prompt for a username and a path to copy to your local machine.  The prompts are in [playbook.yml](../../../playbook.yml)

1. Set server hostname
2. Set hostname to 127.0.0.1
3. Install openvpn and dependencies if they are missing.
4. Clone easyrsa
5. Copy the easyrsa vars conf file (this includes the certificate details (common name, location, etc)
6. Initialize easyrsa. Creates directory structure and index
7. Build the certificate authority
8. Generate the server certificate pair
9. Generate diffie hellman secret.  
10. Create the ta.key to use tls-authentication.
11. Copy the sysctl.conf file and restart the service.  This enables ipv4 forwarding
12. Copy the openvpn server.conf and start the openvpn service.  
13. The include module allows to run more playbooks

## newuser.yml
1. Check to see if user is easyrsa index
2. If user is index, the playbook will fail and stop. 
3. Generate client certificate pair
4. Copy the generated files to /home/ubuntu.  This is done in order to use the Ansible fetch module.  If we try to fetch the files in /etc/openvpn/, it would require using sudo.  Sudo would be executed on local machine as well and there's no way to input your local sudo password during an Ansible run.
5. Generate the client configuration and put it in `/home/ubuntu`
6. Copy files in `/home/ubuntu` to user's local path
7. Once files are copied, the client key pairs will be cleaned up and `/home/ubuntu`
