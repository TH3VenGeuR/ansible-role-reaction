ansible-role-reaction
=========

Ansible role to Install reaction, an alternative to fail2ban written in go

Documentation 
-------------

Please see the official git repository https://framagit.org/ppom/reaction to define some variable (e.g. version)
By default, ssh is protected.

Role Variables
--------------

reaction_version: the version of reaction to install, default 1.1.2
```bash
reaction_version: 1.1.2
```
reaction_streams: a dictionary that define how to protect the server. "name", "command" and "regex" are mandatory. "retry", "retryperiod", "action" are optional because the have default value.

```bash
reaction_streams:
  - name: nginx
    command: ['tail','-n0','-f','/var/log/nginx/access.log']
    regex: '^<ip> .* "POST /auth/login HTTP/..." 401 [0-9]+ .https://mydomain.com'
    retry: 3
    retryperiod: 1h
    actions: banFor('6h')
 
```

Dependencies
------------

Example Playbook
----------------

Including an example of how to use your role (for instance, with variables passed in as parameters) is always nice for users too:

    - hosts: all
      roles:
         - { role: ansible-role-reaction }

License
-------

GPL3
