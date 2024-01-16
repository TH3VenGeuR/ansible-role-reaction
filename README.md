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
You can choose if you want to disable ssh protection with reaction_protect_ssh to false. Some distribution set a different name the ssh service, you can define it with variable reaction_ssh_servicename.

```bash
reaction_protect_ssh: true
reaction_ssh_servicename: ssh
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
  - name: sshd
    command: ['journalctl', '-n0', '-fu', 'ssh.service']
    regex: 'authentication failure;.*rhost=<ip>' 
```

Compatibility
------------
Role has been tested on : 
- Debian 12
- Rocky 8

Example Playbook
----------------

Including an example of how to use your role (for instance, with variables passed in as parameters) is always nice for users too:

    - hosts: all
      roles:
         - { role: ansible-role-reaction }

License
-------

GPL3
