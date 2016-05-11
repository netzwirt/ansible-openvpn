# ansible-openvpn

OpenVPN with PKI for Ubuntu/Debian

## Requirements

None

## Role Variables

Certificate subject:

	openvpn_domainComponent_tld: "com"
	openvpn_domainComponent_domain: "example"
	openvpn_organizationName: "ACME Inc"
	
Create a list with all users you wish to have access to the vpn server:

	openvpn_users:
	- { username: 'fred', fullname: 'Fred Flintstone', email: 'fred@example.com' }
	- { username: 'john', fullname: 'John Example', email: 'john@example.com' }

Revoke certificates:

Do **not** delete user from `openvpn_users`. To revoke certificates create a list containing usernames:

	openvpn_revocation_list:
	- fred

Server Pool configuration

The simple method: This will use openvpn's --server parameter

    openvpn_server_full_pool: "10.8.0.0 255.255.255.0"
    

The advanced method: Take control over ifconfig-pool. Set `openvpn_server_full_pool` to empty string and configure route, ifconfig-pool and ifconfig

    openvpn_server_full_pool: ''
    openvpn_ifconfig: "10.8.8.1 255.255.252.0"
    openvpn_push_route_gateway: "10.8.8.1"
    openvpn_ifconfig_pool: "10.8.8.2 10.8.8.199 255.255.255.0"

## Renew certificates from command-line

Pass extra variable `simplepki_renew_certificates` to `ansible-playbook`. This variable should only be passed as command line argument.

	ansible-playbook --extra-vars '{"simplepki_renew_certificates": ["fred","john"]}'

## Revoke certificates from command-line

Pass extra variable `simplepki_revocation_list` to `ansible-playbook`.

    ansible-playbook --extra-vars '{"simplepki_revocation_list": ["fred","john"]}'

## Dependencies

- netzwirt.simple-pki in the playbook

# Example Playbook

    ---  
    - hosts: openvpn
      become: true
      roles:
      - netzwirt.simple-pki
      - netzwirt.openvpn

# License

BSD

# Author Information

[netzwirt](https://github.com/netzwirt)
