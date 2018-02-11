[![Build Status](https://travis-ci.org/while-true-do/ansible-role-monit.svg?branch=master)](https://travis-ci.org/while-true-do/ansible-role-monit)

# Ansible Role: Monit
| A role to install and configure [Monit](https://mmonit.com/monit/).

- will do the basic configuration
- adds some default checks
- adds pam authentication for the Web Server

## Motivation

[Monit](https://mmonit.com/monit/) is a simple and easy to use monitoring daemon.
It can run locally to monitor daemons, files, logs and more. You can also use it to monitor remote ports.

You can add more checks with another role. Please see the example below.

## Installation

Install from [Ansible Galaxy](https://galaxy.ansible.com/while-true-do/monit)

```
ansible-galaxy install while-true-do.monit
```

Install from [Github](https://github.com/while-true-do/ansible-role-monit)

```
git clone https://github.com/while-true-do/ansible-role-monit.git while-true-do.monit
```

## Requirements

Used Modules:

-   [package_module](http://docs.ansible.com/ansible/latest/package_module.html)
-   [template_module](http://docs.ansible.com/ansible/latest/template_module.html)
-   [file_module](http://docs.ansible.com/ansible/latest/file_module.html)
-   [service_module](http://docs.ansible.com/ansible/latest/service_module.html)
-   [include_tasks_module](https://docs.ansible.com/ansible/latest/include_tasks_module.html)

## Dependencies

None.

## Role Variables

```yaml
---
# defaults/main.yml for monit
# You can set the state to ["present"|"absent"|"latest"]
wtd_monit_state: "present"
wtd_monit_packages: ["monit"]

# Force reload monit (useful for roles, that are creating their own checks)
# or, if you are creating checks on your own.
wtd_monit_reload_checks: False

# Configure Monit
# For more information about the values, please consult the monitrc file.
#
# Set the check interval
wtd_monit_daemon: 30
wtd_monit_start_delay: 240

wtd_monit_logfile: "syslog"
wtd_monit_pidfile: ""
wtd_monit_idfile: ""
wtd_monit_statefile: ""

wtd_monit_mailserver: ""
wtd_monit_eventqueue: ""
wtd_monit_mmonit: ""
wtd_monit_alerts: []

# Configure Monit Webserver
wtd_monit_webserver_state: True

wtd_monit_webserver_port: 2812
wtd_monit_webserver_address: "localhost"
wtd_monit_webserver_signature: "disable"
wtd_monit_webserver_ssl: False
wtd_monit_webserver_pemfile: ""

# Allow sudoers to connect (works only, if pam is true)
# You can also use multiple options.
wtd_monit_webserver_allows: ["@wheel"]
# Write the pam config for monit webserver
# FIXME: A valid while-true-do.pam role might be an idea
wtd_monit_webserver_pam: True

# Preconfigure some default checks
wtd_monit_default_checks: True
```

## Example Playbook

Simple Example:

```yaml
- hosts: servers 
  roles:
    - { role: while-true-do.monit }
```

Advanced Example:

```yaml
- hosts: servers 
  roles:
    - { role: while-true-do.monit, wtd_monit_default_checks: False, wtd_monit_webserver_state: False }
```

Usage in other roles:

```
# defaults/main.yml
# Vars for dependency role while-true-do.monit
wtd_monit_reload_checks: True
```

```
# tasks/main.yml
- name: Create Monit Check
  template:
    src: foo.j2
    dest: /etc/monit.d/foo
    owner: root
    group: root
    mode: 0644
  tags:
    - monit
```

## Testing

All tests are located in [test directory](./tests/).

Basic testing:

```
bash ./tests/test-spelling.sh
bash ./tests/test-ansible.sh
```

## Contribute / Bugs

Thank you so much for considering to contribute. Every contribution helps us.
We are really happy, when somebody is joining the hard work. Please have a look 
at the links first.

-   [Contribution Guidelines](./docs/CONTRIBUTING.md)
-   [Create an issue or Request](https://github.com/while-true-do/ansible-role-monit/issues)
-   [See who was contributing already](https://github.com/while-true-do/ansible-role-monit/graphs/contributors)

## License

This work is licensed under a [BSD License](https://opensource.org/licenses/BSD-3-Clause).

## Author Information

Blog: [blog.while-true-do.org](https://blog.while-true-do.org)

Mail: [hello@while-true-do.org](mailto:hello@while-true-do.org)
