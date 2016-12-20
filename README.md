# Ansible: openvpn

Installation and Configuring of OpenVPN

Available on Ansible Galaxy: [pgkehle.openvpn](https://galaxy.ansible.com/pgkehle/openvpn)

## Variables

Host Definitions typically contain the following:

```
portals                 Ansible group of servers that are configured as OpenVPN portals

target_servers          Listing of servers that are directed through the server
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

Flags for which sections to run
```
do_server_config:   Server Configuration/Initialization
```

## Examples

```YAML

  - hosts: all
 
    vars: 

    # for db_create, db_create_config is a YAML file that holds information about all databases that need
    # to be created.  Required format is as follows (with typical roles displayed):
    #
    #  ---
    # app_users:
    #  - { db_name: "", user: "", pwd: "", roles: ["readWrite", "userAdmin"] } 

    target_servers: []
    dns_servers:    [] 
    
    server_init:    true
    
    roles:
      - { name: pgkehle.openvpn, server_init: true }
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
