# Ansible Role: snort

This Ansible Role builds and installs the [snort](https://github.com/v0l/snort) Typescript frontend assets. It is intended to be composed with a separate role for the web proxy configuration.

Tested on:

* Archlinux
* Ubuntu 22.04

## Requirements

Install node anyway you like, or let this role do it for you:

* [ansible-role-nodejs](https://github.com/bleetube/ansible-role-nodejs) 

`requirements.yml`:

```yaml
roles:
  - src: https://github.com/bleetube/ansible-role-nodejs
    name: bleetube.nodejs
```

It will set up node, npm, yarn, and n using the nodesource Debian repositories.

## Dependencies

* [nginx_conf](docs/examples/nginx_conf.yml) (optional)

## Role Variables

See the role [defaults](defaults/main.yml). For a working example, see this [homelab stack](https://github.com/bleetube/satstack).

## Example Playbook

This role should not be run as root.

```yaml
- hosts: snort
  roles:
    - role: nginxinc.nginx_core.nginx
      become: yes
    - role: bleetube.nodejs
      become: yes
      tags: nodejs
    - role: bleetube.snort
      tags: snort
  tasks:
    - import_tasks: nginx_conf.yml
      become: yes
```
