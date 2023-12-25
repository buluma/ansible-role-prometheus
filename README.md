# Ansible role [prometheus](https://galaxy.ansible.com/ui/standalone/roles/buluma/prometheus/documentation)

Install and configure Prometheus

|GitHub|Version|Issues|Pull Requests|Downloads|
|------|-------|------|-------------|---------|
|[![github](https://github.com/buluma/ansible-role-prometheus/actions/workflows/molecule.yml/badge.svg)](https://github.com/buluma/ansible-role-prometheus/actions/workflows/molecule.yml)|[![Version](https://img.shields.io/github/release/buluma/ansible-role-prometheus.svg)](https://github.com/buluma/ansible-role-prometheus/releases/)|[![Issues](https://img.shields.io/github/issues/buluma/ansible-role-prometheus.svg)](https://github.com/buluma/ansible-role-prometheus/issues/)|[![PullRequests](https://img.shields.io/github/issues-pr-closed-raw/buluma/ansible-role-prometheus.svg)](https://github.com/buluma/ansible-role-prometheus/pulls/)|[![Ansible Role](https://img.shields.io/ansible/role/d/buluma/prometheus)](https://galaxy.ansible.com/ui/standalone/roles/buluma/prometheus/documentation)|

## [Example Playbook](#example-playbook)

This example is taken from [`molecule/default/converge.yml`](https://github.com/buluma/ansible-role-prometheus/blob/master/molecule/default/converge.yml) and is tested on each push, pull request and release.

```yaml
---
- name: Converge
  hosts: all
  become: yes
  gather_facts: yes

  roles:
    - role: buluma.prometheus
```

The machine needs to be prepared. In CI this is done using [`molecule/default/prepare.yml`](https://github.com/buluma/ansible-role-prometheus/blob/master/molecule/default/prepare.yml):

```yaml
---
- name: Prepare
  hosts: all
  become: yes
  gather_facts: yes

  pre_tasks:
    - name: Redhat | subscription-manager register
      ansible.builtin.raw: |
        set -eu
        subscription-manager register \
          --username={{ lookup('env', 'REDHAT_USERNAME') }} \
          --password={{ lookup('env', 'REDHAT_PASSWORD') }} \
          --auto-attach
      changed_when: false
      failed_when: false

    - name: Debian | apt-get install python3
      ansible.builtin.raw: |
        set -eu
        apt-get update
        DEBIAN_FRONTEND=noninteractive apt-get install -y python3
      changed_when: false
      failed_when: false

    - name: Redhat | yum install python3
      ansible.builtin.raw: |
        set -eu
        yum makecache
        yum install -y python3
      changed_when: false
      failed_when: false

    - name: Suse | zypper install python3
      ansible.builtin.raw: |
        set -eu
        zypper -n --gpg-auto-import-keys refresh
        zypper -n install -y python3
      changed_when: false
      failed_when: false

    - name: Cp -rfT /etc/skel /root
      ansible.builtin.raw: |
        set -eu
        cp -rfT /etc/skel /root
      changed_when: false
      failed_when: false

    - name: Setenforce 0
      ansible.builtin.raw: |
        set -eu
        setenforce 0
        sed -i 's/^SELINUX=.*$/SELINUX=permissive/g' /etc/selinux/config
      changed_when: false
      failed_when: false

    - name: Systemctl stop firewalld.service
      ansible.builtin.raw: |
        set -eu
        systemctl stop firewalld.service
        systemctl disable firewalld.service
      changed_when: false
      failed_when: false

    - name: Systemctl stop ufw.service
      ansible.builtin.raw: |
        set -eu
        systemctl stop ufw.service
        systemctl disable ufw.service
      changed_when: false
      failed_when: false

    - name: Debian | apt-get install *.deb
      ansible.builtin.raw: |
        set -eu
        DEBIAN_FRONTEND=noninteractive apt-get install -y bzip2 ca-certificates curl gcc gnupg gzip hostname iproute2 passwd procps python3 python3-apt python3-jmespath python3-lxml python3-pip python3-setuptools python3-jmespath python3-venv python3-virtualenv python3-wheel rsync sudo tar unzip util-linux xz-utils zip
      when: ansible_os_family | lower == "debian"
      changed_when: false
      failed_when: false

    - name: Fedora | yum install *.rpm
      ansible.builtin.raw: |
        set -eu
        yum install -y bzip2 ca-certificates curl gcc gnupg2 gzip hostname iproute procps-ng python3 python3-dnf-plugin-versionlock python3-jmespath python3-libselinux python3-lxml python3-pip python3-setuptools python3-jmespath python3-virtualenv python3-wheel rsync shadow-utils sudo tar unzip util-linux xz yum-utils zip
      when: ansible_distribution | lower == "fedora"
      changed_when: false
      failed_when: false

    - name: Redhat-9 | yum install *.rpm
      ansible.builtin.raw: |
        set -eu
        yum-config-manager --enable crb || echo $?
        yum-config-manager --enable codeready-builder-beta-for-rhel-9-x86_64-rpms || echo $?
        yum install -y http://dl.fedoraproject.org/pub/epel/epel-release-latest-9.noarch.rpm
        yum install -y bzip2 ca-certificates curl gcc gnupg2 gzip hostname iproute procps-ng python3 python3-dnf-plugin-versionlock python3-jmespath python3-libselinux python3-lxml python3-pip python3-setuptools python3-jmespath python3-virtualenv python3-wheel rsync shadow-utils sudo tar unzip util-linux xz yum-utils zip
      when: ansible_os_family | lower == "redhat" and ansible_distribution_major_version | lower == "9"
      changed_when: false
      failed_when: false

    - name: Redhat-8 | yum install *.rpm
      ansible.builtin.raw: |
        set -eu
        yum install -y http://dl.fedoraproject.org/pub/epel/epel-release-latest-8.noarch.rpm
        yum install -y bzip2 ca-certificates curl gcc gnupg2 gzip hostname iproute procps-ng python3 python3-dnf-plugin-versionlock python3-jmespath python3-libselinux python3-lxml python3-pip python3-setuptools python3-jmespath python3-virtualenv python3-wheel rsync shadow-utils sudo tar unzip util-linux xz yum-utils zip
      when: ansible_os_family | lower == "redhat" and ansible_distribution_major_version | lower == "8"
      changed_when: false
      failed_when: false

    - name: Redhat-7 | yum install *.rpm
      ansible.builtin.raw: |
        set -eu
        subscription-manager repos --enable=rhel-7-server-optional-rpms || echo $?
        yum install -y http://dl.fedoraproject.org/pub/epel/epel-release-latest-7.noarch.rpm
        yum install -y bzip2 ca-certificates curl gcc gnupg2 gzip hostname iproute procps-ng python3 python3-jmespath python3-libselinux python3-lxml python3-pip python3-setuptools python3-jmespath python3-virtualenv python3-wheel rsync shadow-utils sudo tar unzip util-linux xz yum-plugin-versionlock yum-utils zip
      when: ansible_os_family | lower == "redhat" and ansible_distribution_major_version | lower == "7"
      changed_when: false
      failed_when: false

    - name: Install jmespath python package
      ansible.builtin.pip:
        name: jmespath

  roles:
    - role: buluma.bootstrap
    - role: buluma.epel
    # - role: buluma.core_dependencies
    # - role: buluma.buildtools
    # - role: buluma.setuptools
    - role: buluma.pip
    - role: buluma.python_pip
      python_pip_modules:
        - name: jmespath
```

Also see a [full explanation and example](https://buluma.github.io/how-to-use-these-roles.html) on how to use these roles.

## [Role Variables](#role-variables)

The default values for the variables are set in [`defaults/main.yml`](https://github.com/buluma/ansible-role-prometheus/blob/master/defaults/main.yml):

```yaml
---
prometheus_version: 2.27.0
prometheus_binary_local_dir: ''
prometheus_skip_install: false

prometheus_config_dir: /etc/prometheus
prometheus_db_dir: /var/lib/prometheus
prometheus_read_only_dirs: []

prometheus_web_listen_address: "0.0.0.0:9090"
prometheus_web_external_url: ''
# See https://github.com/prometheus/exporter-toolkit/blob/master/docs/web-configuration.md
prometheus_web_config:
  tls_server_config: {}
  http_server_config: {}
  basic_auth_users: {}

prometheus_storage_retention: "30d"
# Available since Prometheus 2.7.0
# [EXPERIMENTAL] Maximum number of bytes that can be stored for blocks. Units
# supported: KB, MB, GB, TB, PB.
prometheus_storage_retention_size: "0"

prometheus_config_flags_extra: {}
# prometheus_config_flags_extra:
#   storage.tsdb.retention: 15d
#   alertmanager.timeout: 10s

prometheus_alertmanager_config: []
# prometheus_alertmanager_config:
#   - scheme: https
#     path_prefix: alertmanager/
#     basic_auth:
#       username: user
#       password: pass
#     static_configs:
#       - targets: ["127.0.0.1:9093"]
#     proxy_url: "127.0.0.2"

prometheus_alert_relabel_configs: []
# prometheus_alert_relabel_configs:
#   - action: labeldrop
#     regex: replica

prometheus_global:
  scrape_interval: 15s
  scrape_timeout: 10s
  evaluation_interval: 15s

prometheus_remote_write: []
# prometheus_remote_write:
#   - url: https://dev.kausal.co/prom/push
#     basic_auth:
#       password: FOO

prometheus_remote_read: []
# prometheus_remote_read:
#   - url: https://demo.cloudalchemy.org:9201/read
#     basic_auth:
#       password: FOO

prometheus_external_labels:
  environment: "{{ ansible_fqdn | default(ansible_host) | default(inventory_hostname) }}"

prometheus_targets: {}
#  node:
#    - targets:
#        - localhost:9100
#      labels:
#        env: test

prometheus_scrape_configs:
  - job_name: "prometheus"
    metrics_path: "{{ prometheus_metrics_path }}"
    static_configs:
      - targets:
          - "{{ ansible_fqdn | default(ansible_host) | default('localhost') }}:9090"
  - job_name: "node"
    file_sd_configs:
      - files:
          - "{{ prometheus_config_dir }}/file_sd/node.yml"

# Alternative config file name, searched in ansible templates path.
prometheus_config_file: 'prometheus.yml.j2'

prometheus_alert_rules_files:
  - prometheus/rules/*.rules

prometheus_static_targets_files:
  - prometheus/targets/*.yml
  - prometheus/targets/*.json

prometheus_alert_rules:
  - alert: Watchdog
    expr: vector(1)
    for: 10m
    labels:
      severity: warning
    annotations:
      description: "This is an alert meant to ensure that the entire alerting pipeline is functional.\nThis alert is always firing, therefore it should always be firing in Alertmanager\nand always fire against a receiver. There are integrations with various notification\nmechanisms that send a notification when this alert is not firing. For example the\n\"DeadMansSnitch\" integration in PagerDuty."
      summary: 'Ensure entire alerting pipeline is functional'
  - alert: InstanceDown
    expr: 'up == 0'
    for: 5m
    labels:
      severity: critical
    annotations:
      description: '{% raw %}{{ $labels.instance }} of job {{ $labels.job }} has been down for more than 5 minutes.{% endraw %}'
      summary: '{% raw %}Instance {{ $labels.instance }} down{% endraw %}'
  - alert: RebootRequired
    expr: 'node_reboot_required > 0'
    labels:
      severity: warning
    annotations:
      description: '{% raw %}{{ $labels.instance }} requires a reboot.{% endraw %}'
      summary: '{% raw %}Instance {{ $labels.instance }} - reboot required{% endraw %}'
  - alert: NodeFilesystemSpaceFillingUp
    annotations:
      description: '{% raw %}Filesystem on {{ $labels.device }} at {{ $labels.instance }} has only {{ printf "%.2f" $value }}% available space left and is filling up.{% endraw %}'
      summary: 'Filesystem is predicted to run out of space within the next 24 hours.'
    expr: "(\n  node_filesystem_avail_bytes{job=\"node\",fstype!=\"\"} / node_filesystem_size_bytes{job=\"node\",fstype!=\"\"} * 100 < 40\nand\n  predict_linear(node_filesystem_avail_bytes{job=\"node\",fstype!=\"\"}[6h], 24*60*60) < 0\nand\n  node_filesystem_readonly{job=\"node\",fstype!=\"\"} == 0\n)\n"
    for: 1h
    labels:
      severity: warning
  - alert: NodeFilesystemSpaceFillingUp
    annotations:
      description: '{% raw %}Filesystem on {{ $labels.device }} at {{ $labels.instance }} has only {{ printf "%.2f" $value }}% available space left and is filling up fast.{% endraw %}'
      summary: 'Filesystem is predicted to run out of space within the next 4 hours.'
    expr: "(\n  node_filesystem_avail_bytes{job=\"node\",fstype!=\"\"} / node_filesystem_size_bytes{job=\"node\",fstype!=\"\"} * 100 < 20\nand\n  predict_linear(node_filesystem_avail_bytes{job=\"node\",fstype!=\"\"}[6h], 4*60*60) < 0\nand\n  node_filesystem_readonly{job=\"node\",fstype!=\"\"} == 0\n)\n"
    for: 1h
    labels:
      severity: critical
  - alert: NodeFilesystemAlmostOutOfSpace
    annotations:
      description: '{% raw %}Filesystem on {{ $labels.device }} at {{ $labels.instance }} has only {{ printf "%.2f" $value }}% available space left.{% endraw %}'
      summary: 'Filesystem has less than 5% space left.'
    expr: "(\n  node_filesystem_avail_bytes{job=\"node\",fstype!=\"\"} / node_filesystem_size_bytes{job=\"node\",fstype!=\"\"} * 100 < 5\nand\n  node_filesystem_readonly{job=\"node\",fstype!=\"\"} == 0\n)\n"
    for: 1h
    labels:
      severity: warning
  - alert: NodeFilesystemAlmostOutOfSpace
    annotations:
      description: '{% raw %}Filesystem on {{ $labels.device }} at {{ $labels.instance }} has only {{ printf "%.2f" $value }}% available space left.{% endraw %}'
      summary: 'Filesystem has less than 3% space left.'
    expr: "(\n  node_filesystem_avail_bytes{job=\"node\",fstype!=\"\"} / node_filesystem_size_bytes{job=\"node\",fstype!=\"\"} * 100 < 3\nand\n  node_filesystem_readonly{job=\"node\",fstype!=\"\"} == 0\n)\n"
    for: 1h
    labels:
      severity: critical
  - alert: NodeFilesystemFilesFillingUp
    annotations:
      description: '{% raw %}Filesystem on {{ $labels.device }} at {{ $labels.instance }} has only {{ printf "%.2f" $value }}% available inodes left and is filling up.{% endraw %}'
      summary: 'Filesystem is predicted to run out of inodes within the next 24 hours.'
    expr: "(\n  node_filesystem_files_free{job=\"node\",fstype!=\"\"} / node_filesystem_files{job=\"node\",fstype!=\"\"} * 100 < 40\nand\n  predict_linear(node_filesystem_files_free{job=\"node\",fstype!=\"\"}[6h], 24*60*60) < 0\nand\n  node_filesystem_readonly{job=\"node\",fstype!=\"\"} == 0\n)\n"
    for: 1h
    labels:
      severity: warning
  - alert: NodeFilesystemFilesFillingUp
    annotations:
      description: '{% raw %}Filesystem on {{ $labels.device }} at {{ $labels.instance }} has only {{ printf "%.2f" $value }}% available inodes left and is filling up fast.{% endraw %}'
      summary: 'Filesystem is predicted to run out of inodes within the next 4 hours.'
    expr: "(\n  node_filesystem_files_free{job=\"node\",fstype!=\"\"} / node_filesystem_files{job=\"node\",fstype!=\"\"} * 100 < 20\nand\n  predict_linear(node_filesystem_files_free{job=\"node\",fstype!=\"\"}[6h], 4*60*60) < 0\nand\n  node_filesystem_readonly{job=\"node\",fstype!=\"\"} == 0\n)\n"
    for: 1h
    labels:
      severity: critical
  - alert: NodeFilesystemAlmostOutOfFiles
    annotations:
      description: '{% raw %}Filesystem on {{ $labels.device }} at {{ $labels.instance }} has only {{ printf "%.2f" $value }}% available inodes left.{% endraw %}'
      summary: 'Filesystem has less than 5% inodes left.'
    expr: "(\n  node_filesystem_files_free{job=\"node\",fstype!=\"\"} / node_filesystem_files{job=\"node\",fstype!=\"\"} * 100 < 5\nand\n  node_filesystem_readonly{job=\"node\",fstype!=\"\"} == 0\n)\n"
    for: 1h
    labels:
      severity: warning
  - alert: NodeFilesystemAlmostOutOfFiles
    annotations:
      description: '{% raw %}Filesystem on {{ $labels.device }} at {{ $labels.instance }} has only {{ printf "%.2f" $value }}% available inodes left.{% endraw %}'
      summary: 'Filesystem has less than 3% inodes left.'
    expr: "(\n  node_filesystem_files_free{job=\"node\",fstype!=\"\"} / node_filesystem_files{job=\"node\",fstype!=\"\"} * 100 < 3\nand\n  node_filesystem_readonly{job=\"node\",fstype!=\"\"} == 0\n)\n"
    for: 1h
    labels:
      severity: critical
  - alert: NodeNetworkReceiveErrs
    annotations:
      description: '{% raw %}{{ $labels.instance }} interface {{ $labels.device }} has encountered {{ printf "%.0f" $value }} receive errors in the last two minutes.{% endraw %}'
      summary: 'Network interface is reporting many receive errors.'
    expr: "increase(node_network_receive_errs_total[2m]) > 10\n"
    for: 1h
    labels:
      severity: warning
  - alert: NodeNetworkTransmitErrs
    annotations:
      description: '{% raw %}{{ $labels.instance }} interface {{ $labels.device }} has encountered {{ printf "%.0f" $value }} transmit errors in the last two minutes.{% endraw %}'
      summary: 'Network interface is reporting many transmit errors.'
    expr: "increase(node_network_transmit_errs_total[2m]) > 10\n"
    for: 1h
    labels:
      severity: warning
  - alert: NodeHighNumberConntrackEntriesUsed
    annotations:
      description: '{% raw %}{{ $value | humanizePercentage }} of conntrack entries are used{% endraw %}'
      summary: 'Number of conntrack are getting close to the limit'
    expr: "(node_nf_conntrack_entries / node_nf_conntrack_entries_limit) > 0.75\n"
    labels:
      severity: warning
  - alert: NodeClockSkewDetected
    annotations:
      message: '{% raw %}Clock on {{ $labels.instance }} is out of sync by more than 300s. Ensure NTP is configured correctly on this host.{% endraw %}'
      summary: 'Clock skew detected.'
    expr: "(\n  node_timex_offset_seconds > 0.05\nand\n  deriv(node_timex_offset_seconds[5m]) >= 0\n)\nor\n(\n  node_timex_offset_seconds < -0.05\nand\n  deriv(node_timex_offset_seconds[5m]) <= 0\n)\n"
    for: 10m
    labels:
      severity: warning
  - alert: NodeClockNotSynchronising
    annotations:
      message: '{% raw %}Clock on {{ $labels.instance }} is not synchronising. Ensure NTP is configured on this host.{% endraw %}'
      summary: 'Clock not synchronising.'
    expr: "min_over_time(node_timex_sync_status[5m]) == 0\n"
    for: 10m
    labels:
      severity: warning
```

## [Requirements](#requirements)

- pip packages listed in [requirements.txt](https://github.com/buluma/ansible-role-prometheus/blob/master/requirements.txt).

## [State of used roles](#state-of-used-roles)

The following roles are used to prepare a system. You can prepare your system in another way.

| Requirement | GitHub | Version |
|-------------|--------|--------|
|[buluma.bootstrap](https://galaxy.ansible.com/buluma/bootstrap)|[![Ansible Molecule](https://github.com/buluma/ansible-role-bootstrap/actions/workflows/molecule.yml/badge.svg)](https://github.com/buluma/ansible-role-bootstrap/actions/workflows/molecule.yml)|[![Version](https://img.shields.io/github/release/buluma/ansible-role-bootstrap.svg)](https://github.com/shadowwalker/ansible-role-bootstrap)|
|[buluma.pip](https://galaxy.ansible.com/buluma/pip)|[![Ansible Molecule](https://github.com/buluma/ansible-role-pip/actions/workflows/molecule.yml/badge.svg)](https://github.com/buluma/ansible-role-pip/actions/workflows/molecule.yml)|[![Version](https://img.shields.io/github/release/buluma/ansible-role-pip.svg)](https://github.com/shadowwalker/ansible-role-pip)|
|[buluma.epel](https://galaxy.ansible.com/buluma/epel)|[![Ansible Molecule](https://github.com/buluma/ansible-role-epel/actions/workflows/molecule.yml/badge.svg)](https://github.com/buluma/ansible-role-epel/actions/workflows/molecule.yml)|[![Version](https://img.shields.io/github/release/buluma/ansible-role-epel.svg)](https://github.com/shadowwalker/ansible-role-epel)|
|[buluma.python_pip](https://galaxy.ansible.com/buluma/python_pip)|[![Ansible Molecule](https://github.com/buluma/ansible-role-python_pip/actions/workflows/molecule.yml/badge.svg)](https://github.com/buluma/ansible-role-python_pip/actions/workflows/molecule.yml)|[![Version](https://img.shields.io/github/release/buluma/ansible-role-python_pip.svg)](https://github.com/shadowwalker/ansible-role-python_pip)|

## [Context](#context)

This role is a part of many compatible roles. Have a look at [the documentation of these roles](https://buluma.github.io/) for further information.

Here is an overview of related roles:

![dependencies](https://raw.githubusercontent.com/buluma/ansible-role-prometheus/png/requirements.png "Dependencies")

## [Compatibility](#compatibility)

This role has been tested on these [container images](https://hub.docker.com/u/buluma):

|container|tags|
|---------|----|
|[Ubuntu](https://hub.docker.com/repository/docker/buluma/ubuntu/general)|all|
|[Kali](https://hub.docker.com/repository/docker/buluma/kali/general)|all|
|[Debian](https://hub.docker.com/repository/docker/buluma/debian/general)|all|
|[EL](https://hub.docker.com/repository/docker/buluma/enterpriselinux/general)|7, 8|

The minimum version of Ansible required is 2.12, tests have been done to:

- The previous version.
- The current version.
- The development version.

If you find issues, please register them in [GitHub](https://github.com/buluma/ansible-role-prometheus/issues)

## [Changelog](#changelog)

[Role History](https://github.com/buluma/ansible-role-prometheus/blob/master/CHANGELOG.md)

## [License](#license)

[Apache-2.0](https://github.com/buluma/ansible-role-prometheus/blob/master/LICENSE)

## [Author Information](#author-information)

[Shadow Walker](https://buluma.github.io/)


Template inspired by [Robert de Bock](https://github.com/robertdebock)
