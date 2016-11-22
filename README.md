### This not complete or tested yet.

## OpenVPN

This role will install OpenVPN and configure the server to allow VPN connections.  This configuration routes any request to the Amazon VPC through the VPN tunnel.  For now, this includes 10.0.0.0/24, 10.0.1.0/24, 10.0.2.0/24, 10.0.3.0/24.  DNS is also configured to point to 10.0.0.2 (Amazon's DNS IP) so that endpoints can resolve to the private IP address.  Any non-Amazon related requests will go through the regular internet.  Most OpenVPN setups have all traffic tunnel through the VPN server.  It is ideally built on the idea of a user wanting to encrypt all their traffic.

Other settings, client will disconnect if inactive for 1 hour (I believe this is inactivity through the VPN).

Once configured, a route needs to be setup in the VPC.

Go to VPC > Route Tables > Routes and add

Destination:    10.8.0.0/24
Target:         openvpn instance

This will let the other instances know how to route back to the Openvpn instance and back to the end-user.
### Ansible

#### tags
This playbook was setup with a few tags.  Tags can be passed in when running a playbook if you only want certain tasks to be executed.  

Tags:
* server - This will install/configure OpenVPN server
* update - This option is to update the OpenVPN server.conf only (restarts OpenVPN service as well)
* newuser - If you need to create new client keypairs, run it with this tag.

#### prompts
in [openvpn.yml](../../openvpn.yml) - there are prompts configured.  These will always run even if you are just updating the server.conf.
