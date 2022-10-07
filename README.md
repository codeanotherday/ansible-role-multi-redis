ansible-role-multi-redis
=========

[![CI](https://github.com/codeanotherday/ansible-role-multi-redis/actions/workflows/ci.yml/badge.svg?branch=master)](https://github.com/codeanotherday/ansible-role-multi-redis/actions/workflows/ci.yml)

Install and configure multiple Redis instances on same server.

Requirements
------------

This role was developed and tested on Ubuntu 22.04 only. It should work on other on other ubuntu releases as well.

Role Variables
--------------

All supported(& overridable) variables can be looked up in `defaults/main.yml` redisdefaults dict.

Basic variables supported:
```
redis_name: examplename
redis_port: 6379
redis_bind_interface: 127.0.0.1
redis_confdir: '/etc/redis/'
```

Set up replicas with auth.
```
redis_replicaof: '127.0.0.1 8300'
# below fields are optional and required only when master is password protected with "requirepass"
redis_masterauth: secretpassword
redis_masteruser: legituser
```

Custom mount points for Redis db directory.
redis_name is appended compulsorily to directory path to allow multiple Redis to reside on the same box.
```
redis_dbdir: /var/lib/redis.d/
#translated to /var/lib/redis.d/examplename
```

Install custom Redis package version.
```
redis_package_version: '5:6.0.16-1ubuntu1'
```

Add additional configs not yet supported natively by role.
```
redis_additional_config:
    - "hz 10"
    - "lazyfree-lazy-user-del no"
```

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
Check examples directory.

License
-------

MIT / BSD

Author
-------
Vishal Garg
