# Ansible Role - openvpn

Installation and Configuring of OpenVPN

Available on Ansible Galaxy: [isaackehle.openvpn](https://galaxy.ansible.com/isaackehle/openvpn)

## Variables

The template for the OpenVPN configuration uses the global inventory list. Each inventory file must include the following in order for the host to be included:

```yaml
description:
openvpn:
  subnet: aaa.bbb.ccc.ddd
  netmask: lll.mmm.nnn.ooo
  destination: "client" # or 'server'
```

Note: I found that even though the ansible documentation says that groups.all and groups.ungrouped will give you all of the servers, unless I added my connected systems to a group, the servers were not listed in either group.

Host Definitions typically contain the following:

```bash
# Inventory Group that will be included in dns list Inventory entry must also have an ip_address variable
[dns_servers]
server1.domain.com
server2.domain.com
```

```yaml
# Name of Ansible Inventory Group of servers that are configured as OpenVPN portals (default = 'portals')
vpn_server_group_name: "portals"

cert_config:
  country: US # Country
  locality: Miami # Locality
  state: FL # State
  organization: My Company # Organization
  ou: My Group # Organizational Unit
  email: admin@company.com # Email
  cn: CN # Common name, usually fqdn
  dc: DC # Domain Component, if any
```

## Tags/Flags

I use a system of flags and tags that allow the calling playbook to specify which roles are run.
As an example:

```bash
ansible-playbook playbooks/openvpn.yml --limit portals -e "{'flags': ['server_config']}" -t server_config
```

| Flag                | Purpose                                                         |
| ------------------- | --------------------------------------------------------------- |
| server_init         | Server Initialization                                           |
| server_config       | Server Configuration                                            |
| server_cert_gen     | Server Certificate Generation                                   |
| server_cert_pull    | Server Certificate Pull                                         |
| easy_ovpn_init      | Initialize the Easy OpenVPN directory                           |
| easy_ovpn_push_keys | Push keys to the OpenVPN directory                              |
| easy_ovpn_pull_keys | Pull keys from the OpenVPN directory                            |
| client_cert_gen     | Client Certificate Generation (specify only one portal machine) |
| client_cert_sync    | Client Certificate synchronization between servers              |
| easy_ovpn_kill      | Kill the Easy OpenVPN directory                                 |
| client_deploy       | Install the packages and the certificate on the client          |

## Examples

```yaml
- hosts: all
    vars:
        target_servers: []
    roles:
      - { name: isaackehle.openvpn }
```

```bash
ansible-playbook playbooks/openvpn.yml -e "{'flags': ['server_init']}" -t server_init
ansible-playbook playbooks/openvpn.yml -e "{'flags': ['server_config']}" -t server_config
ansible-playbook playbooks/openvpn.yml -e "{'flags': ['easy_ovpn_init']}" -t easy_ovpn_init
ansible-playbook playbooks/openvpn.yml -e "{'flags': ['easy_ovpn_push_keys']}" -t easy_ovpn_push_keys
ansible-playbook playbooks/openvpn.yml -e "{'flags': ['easy_ovpn_kill']}" -t easy_ovpn_kill
ansible-playbook playbooks/openvpn.yml -e "{'flags': ['server_cert_gen']}" -t server_cert_gen
ansible-playbook playbooks/openvpn.yml -e "{'flags': ['server_cert_pull']}" -t server_cert_pull
ansible-playbook playbooks/openvpn.yml -e "{'flags': ['client_cert_gen'], 'target_host': 'localhost', 'local_hostname': 'myhost.local' }" -t client_cert_gen
ansible-playbook playbooks/openvpn.yml -e "{'flags': ['client_cert_sync']}" -t client_cert_sync
ansible-playbook playbooks/openvpn.yml -e "{'flags': ['client_deploy']}" -t client_deploy
```

## Linting

```bash
yamllint -c yamllint.yaml .
ansible-lint .
```

## License

MIT

## Author Information

Isaac Kehle
@isaackehle ([twitter](https://twitter.com/isaackehle), [github](https://github.com/isaackehle), [linkedin](https://www.linkedin.com/in/isaackehle))
