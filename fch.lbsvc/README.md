# lbsvc
This is a simple role to create a simple Load Balancing Configuration on F5 Load balancer using f5 Provided/Supported Ansible Libraries

## Pre-Requisites
* Ansible up and running (meaning with the following pip'installed: f5-sdk, bigsuds, netaddr)
* Ansible v2.4+
* BigIP already licenced and onboarded

## What does it perform:
* Creates :
- nodes
- pool
- pool members
- redirect VS (using the redirect iRule)
- HTTPS VS (using an already SSL Client Profile)

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
You can either create a vault encrypted file or an encrypted string within your variable files so you do not disclose your confidential data (such as your BigIP password).
(https://docs.ansible.com/ansible/2.4/vault.html)

How to encrypt a complete file?
- Store your passwords in a file - '~/.encrypted_passwords.txt'
- Execute playbook as follows - ansible-vault encrypt <<variable_filename>> --vault-password-file ~/.encrypted_password.txt

How to encrypt a single value?
- encrypt your password using the following command: ansible-vault encrypt_string --vault-id a_password_file 'foobar' --name 'the_secret'
- then use it as a single entry in your variable file:
```
some_foo: !vault |
      $ANSIBLE_VAULT;1.1;AES256
      62313365396662343061393464336163383764373764613633653634306231386433626436623361
      6134333665353966363534333632666535333761666131620a663537646436643839616531643561
      63396265333966386166373632626539326166353965363262633030333630313338646335303630
      3438626666666137650a353638643435666633633964366338633066623234616432373231333331
      6564
```

Decryption is specified when you run the playbook by adding the --ask-vault-pass or --vault-password-file attribute to your command.

For more information refer to: https://docs.ansible.com/ansible/2.4/vault.html

## Credits
https://github.com/F5Networks/f5-ansible
