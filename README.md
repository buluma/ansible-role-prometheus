# [prometheus](#prometheus)

Install and configure Prometheus.

|GitHub|GitLab|Quality|Downloads|Version|
|------|------|-------|---------|-------|
|[![github](https://github.com/buluma/ansible-role-prometheus/workflows/Ansible%20Molecule/badge.svg)](https://github.com/buluma/ansible-role-prometheus/actions)|[![gitlab](https://gitlab.com/buluma/ansible-role-prometheus/badges/master/pipeline.svg)](https://gitlab.com/buluma/ansible-role-prometheus)|[![quality](https://img.shields.io/ansible/quality/)](https://galaxy.ansible.com/buluma/prometheus)|[![downloads](https://img.shields.io/ansible/role/d/)](https://galaxy.ansible.com/buluma/prometheus)|[![Version](https://img.shields.io/github/release/buluma/ansible-role-prometheus.svg)](https://github.com/buluma/ansible-role-prometheus/releases/)|

## [Example Playbook](#example-playbook)

This example is taken from `molecule/default/converge.yml` and is tested on each push, pull request and release.
```yaml
---
- name: Converge
  hosts: all
  become: yes
  gather_facts: yes

  roles:
    - role: buluma.prometheus
```

The machine needs to be prepared. In CI this is done using `molecule/default/prepare.yml`:
```yaml
---
- name: Prepare
  hosts: all
  gather_facts: no
  become: yes

  roles:
    - role: buluma.bootstrap
```


## [Role Variables](#role-variables)

The default values for the variables are set in `defaults/main.yml`:
```yaml
---

#
# variables needed to be defined in user's playbook
#



#
# user-configurable defaults
#

prometheus_user:   prometheus
prometheus_group:  prometheus

prometheus_version:                 2.5.0
prometheus_node_exporter_version:   0.16.0
prometheus_alertmanager_version:    0.15.3

gosu_version: "1.11"
prometheus_go_version:   1.11


prometheus_install_path:         /opt/prometheus
prometheus_config_path:          /etc/prometheus
prometheus_rule_path:            "{{ prometheus_config_path }}/rules"
prometheus_file_sd_config_path:  "{{ prometheus_config_path }}/tgroups"
prometheus_db_path:              /var/lib/prometheus
prometheus_log_path:             /var/log/prometheus
prometheus_pid_path:             /var/run/prometheus
prometheus_alertmanager_db_path: /var/lib/alertmanager


prometheus_download_path: /tmp
prometheus_workdir:       "{{ prometheus_download_path }}/prometheus_workdir"
prometheus_goroot:        "{{ prometheus_workdir }}/go"
prometheus_gopath:        "{{ prometheus_workdir }}/gopath"


prometheus_clean_tarball:     true


# rebuild each time?  (only applied to `git` version)
prometheus_rebuild:           false


#
# derived variables; internal use only.
#


#---- the following vars are handled in tasks/set-role-variables.yml ------

#prometheus_use_systemd: False  # use "systemd" to start/restart service?
```

## [Requirements](#requirements)

- pip packages listed in [requirements.txt](https://github.com/buluma/ansible-role-prometheus/blob/master/requirements.txt).

## [Status of used roles](#status-of-requirements)

The following roles are used to prepare a system. You can prepare your system in another way.

| Requirement | GitHub | GitLab |
|-------------|--------|--------|
|[buluma.bootstrap](https://galaxy.ansible.com/buluma/bootstrap)|[![Build Status GitHub](https://github.com/buluma/ansible-role-bootstrap/workflows/Ansible%20Molecule/badge.svg)](https://github.com/buluma/ansible-role-bootstrap/actions)|[![Build Status GitLab ](https://gitlab.com/buluma/ansible-role-bootstrap/badges/master/pipeline.svg)](https://gitlab.com/buluma/ansible-role-bootstrap)|

## [Context](#context)

This role is a part of many compatible roles. Have a look at [the documentation of these roles](https://buluma.nl/) for further information.

Here is an overview of related roles:
![dependencies](https://raw.githubusercontent.com/buluma/ansible-role-prometheus/png/requirements.png "Dependencies")

## [Compatibility](#compatibility)

This role has been tested on these [container images](https://hub.docker.com/u/buluma):

|container|tags|
|---------|----|
|ubuntu|trusty, xenial, bionic|
|debian|jessie, wheezy|
|el|7|

The minimum version of Ansible required is 2.10, tests have been done to:

- The previous version.
- The current version.
- The development version.



If you find issues, please register them in [GitHub](https://github.com/buluma/ansible-role-prometheus/issues)

## [License](#license)

Apache-2.0

## [Author Information](#author-information)

[Michael Buluma](https://buluma.co.ke/)
