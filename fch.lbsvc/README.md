# lbsvc
This is a simple role to create a simple Load Balancing Configuration on F5 Load balancer using f5 Provided/Supported Ansible Libraries

## Pre-Requisites
* Ansible up and running (meaning with the following pip'installed: f5-sdk, bigsuds, netaddr)
* Ansible v2.4+
* BigIP already licenced and onboarded

## What does it perform:
* Creates :
** nodes
** pool
** pool members
** redirect VS (using the redirect iRule)
** HTTPS VS (using an already SSL Client Profile)

## Variables
Any variables could be added to the "default" variables file () 

```
username: "admin"
password: !vault |
          $ANSIBLE_VAULT;1.1;AES256
          3386163836303830393436623231646338616665363030303639363666656464636639336335
          330346663661386166396432383430313137346436620a636264303263353165313666356238
          6433663032306161303633633438333336343331613439383130306162356466646133373764
          623961326765380a316335313364633362353764343130303636643961373337383163303935
          3162

app_name: "myApp"
pool_name: "{{ app_name }}_pool"
redirect_port: "80"
vip_ip: "10.100.26.143"
vip_port: "443"

pool_members:
- port: "80"
  host: "10.100.26.144"
- port: "80"
  host: "10.100.26.145"

```

## Playbook
```
---
- name: Configure http service
  hosts: prod
  gather_facts: false
  roles:
    - { role: fch.lbsvc }

```

## Inventory

```
[prod]
192.168.1.143
```

## Credential storage

Because this role includes usage of credentials to access your BIG-IP, I recommend that you supply these variables in an ansible-vault encrypted file.

This can be supplied out-of-band of this role

Steps:
- Store your vault password in a file - '~/.vault_pass.txt'
- Execute playbook as follows - ansible-vault encrypt <<variable_filename>> --vault-password-file ~/.vault_pass.txt

For more information refer to: http://docs.ansible.com/ansible/latest/playbooks_vault.html

## Certificate validation
To validate the SSL certificates of the BIG-IP REST API
- set validate_certs: true
- Generate a public private key pair
- Store the public key on BIG-IP (https://support.f5.com/csp/article/K13454#bigipsshdaccept)

## Credits
https://github.com/F5Networks/f5-ansible
