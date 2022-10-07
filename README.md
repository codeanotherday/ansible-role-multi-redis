ansible-role-multi-redis
=========

[![CI](https://github.com/codeanotherday/ansible-role-multi-redis/actions/workflows/ci.yml/badge.svg?branch=master)](https://github.com/codeanotherday/ansible-role-multi-redis/actions/workflows/ci.yml)

Install and configure multiple Redis instances on same server.

Requirements
------------

This role was developed and tested on Ubuntu 22.04 only.

Role Variables
--------------

Note: Information below doesn't concern regular user who just wants to configure redises. For developers, this is important.

defaults/main.yaml contains 2 dicts:
1. redisdefaults
2. instance

Both contain same attributes. This was required because I was unable to figure out how to merge dictionary(with same name) passed from playbook to role.

`redisdefaults` is used to merge defaults with overridden values coming from hostvars or role and then assigned to `instance`.

`instance` is then utilised in configuring everything else.

Dependencies
------------

None.

Example Playbook
----------------

Including an example of how to use your role (for instance, with variables passed in as parameters) is always nice for users too:

    ---
    - hosts: localhost
      vars:
        redis_servers:
          - redis_name: test-instance
            redis_confdir: /etc/redis/
            redis_port: 7379
      remote_user: root
      tasks:
        - name: Install redis
          include_role:
            name: ./multi-redis-ansible
          loop: '{{ redis_servers }}'
          vars:
            instance: '{{ redisdefaults|combine(item) }}'

Instead of making it part of playbook, variables can also be picked up from host_vars to make it as generic as possible.

Supported variables currently
-------
All supported(& overridable) variables can be looked up in defaults/main.yml

redisdefaults dictionary

License
-------

MIT / BSD

Author
-------
Vishal Garg
