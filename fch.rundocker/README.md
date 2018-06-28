# rundocker
This is a simple role to create a simple Load Balancing Configuration on F5 Load balancer using f5 Provided/Supported Ansible Libraries

## Pre-Requisites
* Ansible up and running (meaning with the following pip'installed: f5-sdk, bigsuds, netaddr)
* Ansible v2.4+
* BigIP already licenced and onboarded

## What does it do?
* Gets any container from a local registry or from Docker Hub (here it takes the simple but nice "f5devcentral/f5-demo-app" container
* Loops on every container ports listed to create independant running containers.

## Variables
Any variables could be added to the "vars" variables file ()

```
---
# vars file for fch.rundocker
remote_user: "fchmainy"
docker_name: "f5devcentral/f5-demo-app"
svc_name: "myapp"
internal_port: "80"

```

## Playbook
```
---
- hosts: me
  remote_user: fchmainy
  strategy: debug
  gather_facts: yes

  vars:
    container_ports:
      - "9081"
      - "9082"
      - "9083"

  roles:
    - { role: fch.rundocker, become: yes, myports: "{{ container_ports }}" }

```

## Inventory

```
[me]
127.0.0.1
```

