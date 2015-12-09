ansible-openvpn
===============

OpenVPN with PKI for Ubuntu/Debian

Requirements
------------

None

Role Variables
--------------

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

Renew certificates from command-line
------------------------------------

Pass extra variable `simplepki_renew_certificates` to `ansible-playbook`. This variable should only be passed as command line argument.

	ansible-playbook --extra-vars '{"simplepki_renew_certificates": ["fred","john"]}'

Revoke certificates from command-line
-------------------------------------

Pass extra variable `simplepki_revocation_list` to `ansible-playbook`.

    ansible-playbook --extra-vars '{"simplepki_revocation_list": ["fred","john"]}'

Dependencies
------------

- netzwirt.simple-pki

Example Playbook
----------------

    ---  
    - hosts: openvpn
      become: true
      roles:
      - netzwirt.openvpn-server

License
-------

BSD

Author Information
------------------

[netzwirt](https://github.com/netzwirt)
