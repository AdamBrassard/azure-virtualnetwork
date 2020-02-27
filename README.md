Creating/Updating a VNET in Azure
=========

This role is used to create/update a VNET in Azure, set or purge DNS and address prefixes

Requirements
------------

Ansible Azure Modules

A Resource Group must exist prior to executing this role

<https://docs.ansible.com/ansible/latest/modules/azure_rm_virtualnetwork_module.html>

Role Variables
--------------

``` yaml
# Global Vars always required:
resource_group:
location:
vnet_name:
# Role specific Var required:
vnet_address_prefix: #List of IPv4 address ranges where each is formatted using CIDR notation.
dns_servers:

# purging boolean toggles as supported by Azure
purge_address_prefixes: #to be used with state=present to remove existing address CIDR from the named VNET, Must list desired address prefix to purge
purge_dns_servers: #To be used with state=present (the default) to remove all existing associated DNS servers to the VNET
```

Dependencies
------------

Requires Azure_rm Modules

Example Playbooks
----------------

> Creating the basic VNET with default Azure DNS

``` yaml
---
- name: Azure Playbook to create a VNET
  hosts: localhost
  pre_tasks:
    - name: Setting Desired State
      set_fact:  
        resource_group: "ThisRG"
        location: canadacentral
        vnet_name: "ProdVNET"
        vnet_address_prefix: "172.16.0.0/16"

  connection: local
  roles:
    - azure-virtualnetwork
```

> Creating a VNET with a single CDIR and custom DNS servers

``` yaml
---
- name: Azure Playbook to create a VNET
  hosts: localhost
  pre_tasks:
    - name: Setting Desired State
      set_fact:  
        resource_group: "ThisRG"
        location: canadacentral
        vnet_name: "ProdVNET"
        vnet_address_prefix: "172.16.0.0/16"
        dns_servers:
          - "8.8.8.8"
          - "1.1.1.1"

  connection: local
  roles:
    - azure-virtualnetwork
```

> Creating a VNET with multiple address CDIR and custom DNS

``` yaml
---
- name: Azure Playbook to create a VNET
  hosts: localhost
  pre_tasks:
    - name: Setting Desired State
      set_fact:  
        resource_group: "ThisRG"
        location: canadacentral
        vnet_name: "ProdVNET"
        vnet_address_prefix:
          - "172.16.0.0/16"
          - "192.168.0.0/24"
        dns_servers:
          - "8.8.8.8"
          - "1.1.1.1"

  connection: local
  roles:
    - azure-virtualnetwork
```

> Purging DNS from an existing VNET

``` yaml
---
- name: Azure Playbook to create a VNET
  hosts: localhost
  pre_tasks:
    - name: Setting Desired State
      set_fact:  
        resource_group: "ThisRG"
        location: canadacentral
        vnet_name: "ProdVNET"
        purge_dns_servers: yes

  connection: local
  roles:
    - azure-virtualnetwork
```

> Purging one address prefixe from an existing VNETs

``` yaml
---
- name: Azure Playbook to create a VNET
  hosts: localhost
  pre_tasks:
    - name: Setting Desired State
      set_fact:  
        resource_group: "ThisRG"
        location: canadacentral
        vnet_name: "ProdVNET"
        purge_address_prefixes: yes
        vnet_address_prefix:
          - "172.16.0.0/16"

  connection: local
  roles:
    - azure-virtualnetwork
```

License
-------

MIT

Author Information
------------------

Adam Brassard: Abrass on IRC
