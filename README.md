[![build status](https://github.com/alecunsolo/ansible-role-time/actions/workflows/ci.yml/badge.svg)](https://github.com/alecunsolo/ansible-role-time/actions/workflows/ci.yml)
[![pre-commit](https://img.shields.io/badge/pre--commit-enabled-brightgreen?logo=pre-commit)](https://github.com/pre-commit/pre-commit)

Ansible Role: time
=========
## DISCLAIMER
After [this](https://www.redhat.com/en/blog/furthering-evolution-centos-stream) announcement I will not test on RHEL anymore.

---------
A simple role to set timezone and enable NTP (Chrony). Nothing sophisticated and not very configurable, but it's all I need :)

Requirements
------------

The package `tzdata` must be installed. I think only containers don't have it already installed by default.

Role Variables
--------------

Default values defined in [defaults/main.yml](defaults/main.yml)
```yaml
config_timezone: Etc/UTC
```
The wanted timezone.

```yaml
custom_ntp_server: ~
```
If present, configure a preferred ntp server.

The chrony config file path is different for RedHat and Debian, so it is configured in a distro specific variable file:

```yaml
# Debian
chrony_conf_path: /etc/chrony/chrony.conf
# RedHat
chrony_conf_path: /etc/chrony.conf
```

Dependencies
------------

The role use the `community.general.timezone` module, so the `community.general` collection must be available.

Example Playbook
----------------

```yaml
- hosts: all
  vars:
    config_timezone: Europe/Rome
    custom_ntp_server: ntp.example.com
  pre_tasks:
    - name: Ensure tzdata package is installed.
      ansible.builtin.package:
        name: tzdata
        state: present
  roles:
    - alecunsolo.time
```

License
-------

MIT

Notes
-----

Testing with molecule (including the docker images used) is ~~stolen from~~ heavily inspired by [Jeff Geerling](https://www.jeffgeerling.com/). Watch [his video](https://youtu.be/FaXVZ60o8L8) (and the other ones as well).
