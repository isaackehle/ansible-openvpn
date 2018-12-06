# Ansible Role - openvpn

Installation and Configuring of OpenVPN

Available on Ansible Galaxy: [pgkehle.openvpn](https://galaxy.ansible.com/pgkehle/openvpn)

## Variables

The template for the OpenVPN configuration uses the global inventory list.  Each inventory file must include the following in order for the host to be included:
* subnet
* netmask
* description

Note: I found that even though the ansible documentation says that groups.all and groups.ungrouped will give you all of the servers, unless I added my connected systems to a group, the servers were not listed in either group.


Host Definitions typically contain the following:

```yaml
vpn_server_group_name   Name of Ansible group of servers that are configured as OpenVPN portals (default = 'portals')

dns_servers             Listing of DNS servers that are published when the client is connected

sc_country              Country for the cert                        
sc_locality             Locality for the cert
sc_state                State for the cert
sc_organization         Organization for the cert
sc_ou                   Organizational Unit for the cert
sc_email                Email for the cert
sc_common_name          Common name for the cert, usually fqdn
sc_domain_comp          Domain Component for the cert, if any

```

## Tags/Flags

I use a system of flags and tags that allow the calling playbook to specify which roles are run.
As an example:
```
ansible-playbook playbooks/openvpn.yml --limit portals -e "{'flags': ['server_config']}" -t server_config
```


```yaml
server_init:         Server Initialization
server_config:       Server Configuration
server_cert_gen:     Server Certificate Generation
server_cert_pull:    Server Certificate Pull
easy_ovpn_init:      Initialize the Easy OpenVPN directoryu
client_cert_gen:     Client Certificate Generation (specify only one portal machine)
client_cert_sync:    Client Certificate synchronization between servers
easy_ovpn_kill:      Kill the Easy OpenVPN directoryu
client_deploy:       Install the packages and the certificate on the client
```

## Examples

```yaml
- hosts: all
    vars: 
        target_servers: []
        dns_servers:    [] 
    roles:
      - { name: pgkehle.openvpn }
```

```bash
ansible-playbook playbooks/openvpn.yml -e "{'flags': ['server_init']}" -t server_init
ansible-playbook playbooks/openvpn.yml -e "{'flags': ['server_config']}" -t server_config
ansible-playbook playbooks/openvpn.yml -e "{'flags': ['easy_ovpn_init']}" -t easy_ovpn_init
ansible-playbook playbooks/openvpn.yml -e "{'flags': ['server_cert_gen']}" -t server_cert_gen
ansible-playbook playbooks/openvpn.yml -e "{'flags': ['server_cert_pull']}" -t server_cert_pull
ansible-playbook playbooks/openvpn.yml -e "{'flags': ['client_cert_gen'], 'target_host': 'localhost', 'local_hostname': 'myhost.local' }" -t client_cert_gen
ansible-playbook playbooks/openvpn.yml -e "{'flags': ['client_cert_sync']}" -t client_cert_sync
ansible-playbook playbooks/openvpn.yml -e "{'flags': ['client_deploy']}" -t client_deploy
```
## License

MIT

## Author Information

Paul Kehle
@pgkehle ([twitter](https://twitter.com/pgkehle), [github](https://github.com/pgkehle), [linkedin](https://www.linkedin.com/in/pgkehle))

### References

* https://www.digitalocean.com/community/tutorials/how-to-set-up-an-openvpn-server-on-ubuntu-14-04
* https://www.digitalocean.com/community/tutorials/how-to-set-up-an-openvpn-server-on-ubuntu-16-04
* https://help.ubuntu.com/14.04/serverguide/openvpn.html
* http://superuser.com/questions/457020/openvpn-only-route-a-specific-ip-addresses-through-vpn
* http://www.linuxfunda.com/2013/09/14/how-to-install-and-configure-an-open-vpn-with-nat-server-inside-aws-vpc/
* http://serverfault.com/questions/497438/how-to-reset-ubuntu-12-04-iptables-to-default-without-locking-oneself-out
* http://serverfault.com/questions/631037/how-to-route-only-specific-openvpn-traffic-through-a-openvpn-based-on-ip-filteri

* https://forums.openvpn.net/viewtopic.php?t=16862
* https://community.openvpn.net/openvpn/wiki/Hardening
* https://blog.g3rt.nl/openvpn-security-tips.html
* https://wiki.mozilla.org/Security/Server_Side_TLS
* https://www.privateinternetaccess.com/forum/discussion/3478/pfsense-openvpn-not-connected
