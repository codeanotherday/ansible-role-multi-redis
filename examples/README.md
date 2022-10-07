### How to run the example playbooks

There's two different methods depicted to run same playbook. What changes is from where are variables picked up.
Second one is more organised if you have a lot of hosts to maintain.

#### variables inside playbook itself
```sh
ansible-playbook  ./examples/vars-in-playbook/playbook.yml
```

#### variables picked from anywhere else(host_vars, group_vars, host file) and passed on to playbook
```sh
ansible-playbook  ./examples/vars-from-elsewhere/playbook.yml
```


```sh
systemctl status redis-server-`redis_name`
systemctl status redis-server-example1
```