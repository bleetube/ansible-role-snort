# Ansible Role: snort

This Ansible Role builds and installs [snort](https://github.com/v0l/snort). It is intended to be composed with a separate role for the web proxy configuration.

**Warning**: This role is incomplete. Yarn seems problematic to run via Ansible. The build step must be done manually. After running this role and the nginx configuration, the manual steps are as follows:

```shell
systemctl stop snort
cd /var/www/snort
doas -u snort yarn
doas -u snort yarn build
doas -u snort yarn workspace @snort/app intl-extract
doas -u snort yarn workspace @snort/app intl-compile
systemctl start snort
```

Tested on:

* Archlinux
* Ubuntu 22.04

## Requirements

* `nodejs` - version 20 is fine, 
* `yarn`

You can use [ansible-role-nodejs](https://github.com/bleetube/ansible-role-nodejs) if you want to. Here is an example `requirements.yml` for that:

```yaml
roles:
  - src: https://github.com/bleetube/ansible-role-nodejs
    name: bleetube.nodejs
```

It will set up `node`, `npm`, `yarn`, and `n` using the nodesource Debian repositories. But you can also install those by any other method.

## Dependencies

* [nginx_conf](docs/examples/nginx_conf.yml) (optional)

Any similarly configured web proxy may suffice.

## Role Variables

See the role [defaults](defaults/main.yml). For a working example, see this [homelab stack](https://github.com/bleetube/satstack).

## Example Playbook

```yaml
- hosts: snort
  become: yes
  roles:
    - role: nginxinc.nginx_core.nginx
    - role: bleetube.nodejs
      tags: nodejs
    - role: bleetube.snort
      tags: snort
  tasks:
    - import_tasks: nginx_conf.yml
```
