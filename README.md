3proxy
=========

Setup 3proxy on any (only tested on Debian actually) OS with power of Docker.

Requirements
------------

- Docker - can be installed with `geerlingguy.docker` role
- docker-py - will be installed by this role by default, you probably need to change `proxy_docker_py_pkg` variable if you are running OS different from Debian 10, Debian 11

Role Variables
--------------

```yaml
proxy_config:
  # http proxy port
  proxy_port: 3128
  # socks proxy port
  socks_port: 1080
# list of users
proxy_users:
  - admin:CL:bigsecret
  - test2:CR:$1$lFDGlder$pLRb4cU2D7GAT58YQvY49.
  - test3:NT:BD7DFBF29A93F93C63CB84790DA00E63
# install docker-py package, this package is required for Ansible to communicate with Docker
proxy_docker_py_install: true
# docker-py package name
proxy_docker_py_pkg: python3-docker
# install fail2ban and enable 3proxy jail
proxy_fail2ban_setup: true
# do not ban those ip-s
proxy_fail2ban_ignoreip:
  - 1.2.3.4
```

Managing users
--------------

List of user must be in form like this 

```
user:TYPE:password or hash
```

Where `TYPE` is

- `CL` - cleartext password, pretty self-explanatory
- `CR` - crypt password, only MD5 crypt passwords are supported, you can create hashes with this commands
  
  ```
  $ openssl passwd -1 -salt $(openssl rand -hex 4) yourpassword
  $1$9dacdec9$KogGUDYRAiX2RvwSMDUq11

  $ mycrypt $(openssl rand -hex 4) yourpassword
  CR:$1$e8148d55$7u7btLtBKC2lXdIXKD4j.1
  ```
- `NT` - NT-hashed (MD4) passwords in hex, as used in pwdump or SAMBA 
  
  ```
  $ mycrypt yourpassword
  NT:5DFB7533508B0EA192CCF7F6B64427FC
  ```

Dependencies
------------

- `geerlingguy.docker`

Example Playbook
----------------


```yaml
---
- name: Setup 3proxy
  become: yes
  hosts:
    - proxy
  roles:
    - 3proxy
```

License
-------

MIT
