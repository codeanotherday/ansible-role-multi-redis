ansible-role-multi-redis
=========

[![CI](https://github.com/codeanotherday/ansible-role-multi-redis/actions/workflows/ci.yml/badge.svg?branch=master)](https://github.com/codeanotherday/ansible-role-multi-redis/actions/workflows/ci.yml)

Install and configure multiple Redis servers on same servers.

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

Dependencies
------------

None.

Example Playbook
----------------

---
- hosts: localhost
  become: true
  connection: local
  vars:
    redis_servers:
      - redis_name: example1
        redis_port: 7979
      - redis_name: example2
        redis_port: 7980
  tasks:
    - name: Install redis
      include_role:
        name: ../../../


Instead of making it part of playbook, variables can also be picked up from host_vars to make it as generic as possible.
Check examples directory.

License
-------

MIT / BSD

Author
-------
Vishal Garg
