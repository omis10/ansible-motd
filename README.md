Role Name
=========

An [Ansible] role to manage [MOTD]

Requirements
------------

None

Role Variables
--------------

```
---
# defaults file for ansible-motd
#
# Define any custom messages to append to /etc/motd
motd_custom_messages:
  - line: 'Distribution'
    content: '{{ ansible_distribution }}'
  - line: 'Distribution Release'
    content: '{{ ansible_distribution_release }}'
  - line: 'Distribution Version'
    content: '{{ ansible_distribution_version }}'
  - line: 'DNS Search'
    content: '{{ ansible_dns.search|join (" ") }}'
  - line: 'DNS Server(s)'
    content: '{{ ansible_dns.nameservers|join (" ") }}'
  - line: 'Interfaces'
    content: >-
             {% for int in ansible_interfaces -%}{{ int }}
             {%- if hostvars[inventory_hostname]['ansible_'+int]['ipv4']['address'] is defined -%}
             ({{ hostvars[inventory_hostname]['ansible_'+int]['ipv4']['address'] }})
             {%- endif %}
             {%- if hostvars[inventory_hostname]['ansible_'+int]['macaddress'] is defined -%}
             ({{ hostvars[inventory_hostname]['ansible_'+int]['macaddress'] }})
             {%- endif %}
             {% endfor %}
  - line: 'Kernel'
    content: '{{ ansible_kernel }}'
  # - line: 'LSB'
  #   content: '{{ ansible_lsb.description }}'
  - line: 'Memory Installed'
    content: '{{ (ansible_memtotal_mb / 1024) | round(1) }}GB'
  - line: 'Memory Swapfile'
    content: '{{ (ansible_swaptotal_mb / 1024) | round(1) }}GB'
  - line: 'Mounts'
    content: >-
             {% for mnt in ansible_mounts -%}
             Mount: {{ mnt.device }}({{ mnt.mount }})({{ (mnt.size_total / 1024 / 1024 / 1024)| round(1) }}GB)
             {% endfor %}
  - line: 'Processors'
    content: '{{ ansible_processor_vcpus }}'
  - line: 'Python Version'
    content: '{{ ansible_python_version }}'
  - line: 'Timezone'
    content: '{{ ansible_date_time.tz }}({{ ansible_date_time.tz_offset }})'

# Defines default directory for MOTD scripts
motd_default_dir: '/etc/update-motd.d'

# Defines a default message to apply above any custom messages
motd_default_message: 'Welcome to your Dev Environment'

# Defines if all default MOTD scripts in motd_default_dir should be disabled
motd_disable_defaults: false
```

Dependencies
------------

None

Example Playbook
----------------

```
---
- hosts: all
  become: true
  vars:
  roles:
    - role: ansible-motd
  tasks:
```

License
-------

BSD

Author Information
------------------

Larry Smith Jr.
- @mrlesmithjr
- http://everythingshouldbevirtual.com
- mrlesmithjr [at] gmail.com

[Ansible]: <https://www.ansible.com>
[MOTD]: <https://en.wikipedia.org/wiki/Motd_(Unix)>
