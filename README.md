# bind

[![License](https://img.shields.io/badge/license-New%20BSD-blue.svg?style=flat)](https://raw.githubusercontent.com/saucelabs-ansible/bind/master/LICENSE)
[![Build Status](https://travis-ci.org/saucelabs-ansible/bind.svg?branch=master)](https://travis-ci.org/saucelabs-ansible/bind)

[![Platform](http://img.shields.io/badge/platform-ubuntu-dd4814.svg?style=flat)](#)

[![Project Stats](https://www.openhub.net/p/saucelabs-ansible-bind/widgets/project_thin_badge.gif)](https://www.openhub.net/p/saucelabs-ansible-bind/)

Ansible role to setup [BIND (Berkley Internet Naming Daemon)](https://www.isc.org/downloads/bind/).


## Tests

| Family | Distribution | Version | Test Status |
|:-:|:-:|:-:|:-:|
| Debian | Ubuntu  | Precise | [![x86_64](http://img.shields.io/badge/x86_64-passed-006400.svg?style=flat)](#) |
| Debian | Ubuntu  | Trusty  | [![x86_64](http://img.shields.io/badge/x86_64-passed-006400.svg?style=flat)](#) |


## Requirements

- ansible >= 1.9.4


## Role Variables

- **debug**: flag to run debug tasks (default: false).
- **bind_dir_log**: directory where to store the bind log files.
- **bind_default_resolvconf**: wether or not you want to run resolvconf.
- **bind_default_options**: extra parameters to pass to the bind daemon.
- **bind_named_conf_acl**: content for the `named.conf` `acl` section.
- **bind_named_conf_controls**: content for the `named.conf` `controls` section.
- **bind_named_conf_keys**: keys for the `named.conf` `keys` section
                            (see example below for format).
- **bind_named_conf_logging**: `channels` and `categories` for the `named.conf` `logging` section
                               (see example below for format).
- **bind_named_conf_options**: content for the `named.conf` `options` section (mandatory).

Unless stated otherwise
a default value is provided for each of the variables mentioned above
in `defaults/main.yml`.


## Dependencies

None.


## Playbooks

Example:

    - hosts: servers
      vars:
        debug: yes

        bind_default_options: '-4 -u bind'
        bind_dir_log: /var/log/named

        bind_named_conf_acl:
          trusted: |
            localhost;
            localnets;

        bind_named_conf_controls: |
          inet 127.0.0.1 port 953 allow { 127.0.0.1; };

        bind_named_conf_keys:
          mykey:
            algorithm: hmac-md5
            secret: QJc08cnP1xkoF4a/eSZZbw==
          mykey2:
            algorithm: hmac-md5
            secret: QJc08cnP1xkoF4a/eSZZbw==

        bind_named_conf_logging:
          channels:
            update_debug: |
              file "{{ bind_dir_log }}/update_debug.log" versions 3 size 100k;
              severity debug;
              print-severity  yes;
              print-time      yes;
            security_info: |
              file "{{ bind_dir_log }}/security_info.log" versions 1 size 100k;
              severity info;
              print-severity  yes;
              print-time      yes;
            bind_log: |
              file "{{ bind_dir_log }}/bind.log" versions 3 size 1m;
              severity info;
              print-category  yes;
              print-severity  yes;
              print-time      yes;
          categories:
            default: bind_log
            lame-servers: 'null'
            update: update_debug
            update-security: update_debug
            security: security_info

      roles:
         - role: saucelabs-ansible.bind
           tags: bind


## Tags

- **apparmor**: apparmor configuration tasks.
- **configuration**: configuration tasks.
- **debug**: role variables debug task.
- **installation**: installation tasks.
- **validation**: role variables validation task.

## Test

To run the tests you will need to install:

- [tox](https://tox.readthedocs.org/)
- [vagrant](https://www.vagrantup.com/)

To run all tests against all pre-defined OS/distributions * ansible versions:

```
$ tox
```

To run tests for `trusty64`:

```
$ cd tests
$ bash test_idempotence.sh --box trusty64.vagrant.dev
# log file will be stores under tests/log
```

To perform debugging on a specific environment:

```
$ cd tests
$ vagrant up trusty64.vagrant.dev

# to provision using the test.yml playbook (as many time as you need)
$ vagrant provision trusty64.vagrant.dev

# to access the Vagrant box
$ vagrant ssh trusty64.vagrant.dev
```


## Links

- [Ubuntu 14.04 > Ubuntu Server Guide > Domain Name Service (DNS)](https://help.ubuntu.com/lts/serverguide/dns.html)
- [Debian > Wiki > Bind9](https://wiki.debian.org/Bind9)


## License

BSD


## Author Information

- [esacteksab](https://github.com/esacteksab/)
- [steenzout](https://github.com/steenzout/)
