# [Ansible role alertmanager_docker](#alertmanager_docker)

Installs and configures alertmanager container based on official alertmanager docker container

|GitHub|Downloads|Version|
|------|---------|-------|
|[![github](https://github.com/mullholland/ansible-role-alertmanager_docker/actions/workflows/molecule.yml/badge.svg)](https://github.com/mullholland/ansible-role-alertmanager_docker/actions/workflows/molecule.yml)|[![downloads](https://img.shields.io/ansible/role/d/mullholland/alertmanager_docker)](https://galaxy.ansible.com/mullholland/alertmanager_docker)|[![Version](https://img.shields.io/github/release/mullholland/ansible-role-alertmanager_docker.svg)](https://github.com/mullholland/ansible-role-alertmanager_docker/releases/)|
## [Example Playbook](#example-playbook)

This example is taken from [`molecule/default/converge.yml`](https://github.com/mullholland/ansible-role-alertmanager_docker/blob/master/molecule/default/converge.yml) and is tested on each push, pull request and release.

```yaml
---
- name: Converge
  hosts: all
  become: true
  gather_facts: true
  vars:
    adguardhome_docker_config:
      tls:
        options:
          modern:
            minVersion: "VersionTLS13"
            sniStrict: true

  roles:
    - role: "mullholland.alertmanager_docker"
```

The machine needs to be prepared. In CI this is done using [`molecule/default/prepare.yml`](https://github.com/mullholland/ansible-role-alertmanager_docker/blob/master/molecule/default/prepare.yml):

```yaml
---
- name: Prepare
  hosts: all
  become: true
  gather_facts: true
  vars:
    pip_packages:
      - "docker"

  roles:
    - role: mullholland.docker
    - role: mullholland.repository_epel
    - role: mullholland.pip
```



## [Role Variables](#role-variables)

The default values for the variables are set in [`defaults/main.yml`](https://github.com/mullholland/ansible-role-alertmanager_docker/blob/master/defaults/main.yml):

```yaml
---
# General config
alertmanager_docker_network_name: "web"
alertmanager_docker_base_path: "/opt"
alertmanager_docker_timezone: "Europe/Berlin"

# User/Group of the stack. Everything is mapped to this, instead of root.
alertmanager_docker_user: "homelab"
alertmanager_docker_uid: "900"
alertmanager_docker_group: "homelab"
alertmanager_docker_gid: "900"
alertmanager_docker_user_system: true

# which container version to install
# can also be latest
alertmanager_docker_version: "prom/alertmanager:latest"

# https://docs.victoriametrics.com/alertmanager/#advanced-usage
alertmanager_docker_commands:
  - "--config.file=/etc/alertmanager/alertmanager.yml"
  - "--storage.path=/etc/alertmanager/data"

alertmanager_docker_environment_variables: []
alertmanager_docker_volumes:
  - "{{ alertmanager_docker_base_path }}/alertmanager/config/alertmanager.yml:/etc/alertmanager/alertmanager.yml"
  - "{{ alertmanager_docker_base_path }}/alertmanager/data:/etc/alertmanager/data"
  - "{{ alertmanager_docker_base_path }}/alertmanager/templates:/etc/alertmanager/templates"

# which port to expose. can be empty
alertmanager_docker_ports:
  - "9093:9093"

alertmanager_docker_labels:
  - "traefik.enable=false"
# - "traefik.enable=true"
# - "traefik.http.routers.alertmanager.entryPoints=https"
# - "traefik.http.routers.alertmanager.rule=Host(`alertmanager.example.com`)"
# - "traefik.http.routers.alertmanager.tls.certResolver=letsEncrypt"
# - "traefik.http.routers.alertmanager.tls=true"
#  - "traefik.http.routers.alertmanager.service=alertmanager"
#  - "traefik.http.routers.alertmanager.loadbalancer.server.port=80"


# Variable will be put throuhj `to_nice_yaml` as the config in `alertmanager.yml`
# https://prometheus.io/docs/alerting/latest/configuration/
alertmanager_docker_config:
  templates:
    - '/etc/alertmanager/templates/*.tmpl'
  route:
    receiver: blackhole
  receivers:
    - name: blackhole```

## [Requirements](#requirements)

- pip packages listed in [requirements.txt](https://github.com/mullholland/ansible-role-alertmanager_docker/blob/master/requirements.txt).

## [State of used roles](#state-of-used-roles)

The following roles are used to prepare a system. You can prepare your system in another way.

| Requirement | GitHub | GitLab |
|-------------|--------|--------|
|[mullholland.repository_epel](https://galaxy.ansible.com/mullholland/repository_epel)|[![Build Status GitHub](https://github.com/mullholland/ansible-role-repository_epel/workflows/Ansible%20Molecule/badge.svg)](https://github.com/mullholland/ansible-role-repository_epel/actions)|[![Build Status GitLab](https://gitlab.com/opensourceunicorn/ansible-role-repository_epel/badges/master/pipeline.svg)](https://gitlab.com/opensourceunicorn/ansible-role-repository_epel)|
|[mullholland.docker](https://galaxy.ansible.com/mullholland/docker)|[![Build Status GitHub](https://github.com/mullholland/ansible-role-docker/workflows/Ansible%20Molecule/badge.svg)](https://github.com/mullholland/ansible-role-docker/actions)|[![Build Status GitLab](https://gitlab.com/opensourceunicorn/ansible-role-docker/badges/master/pipeline.svg)](https://gitlab.com/opensourceunicorn/ansible-role-docker)|
|[mullholland.pip](https://galaxy.ansible.com/mullholland/pip)|[![Build Status GitHub](https://github.com/mullholland/ansible-role-pip/workflows/Ansible%20Molecule/badge.svg)](https://github.com/mullholland/ansible-role-pip/actions)|[![Build Status GitLab](https://gitlab.com/opensourceunicorn/ansible-role-pip/badges/master/pipeline.svg)](https://gitlab.com/opensourceunicorn/ansible-role-pip)|

## [Context](#context)

This role is a part of many compatible roles. Have a look at [the documentation of these roles](https://mullholland.net) for further information.

Here is an overview of related roles:
![dependencies](https://raw.githubusercontent.com/mullholland/ansible-role-alertmanager_docker/png/requirements.png "Dependencies")

## [Compatibility](#compatibility)

This role has been tested on these [container images](https://hub.docker.com/u/mullholland):

|container|tags|
|---------|----|
|[EL](https://hub.docker.com/r/mullholland/enterpriselinux)|all|
|[Fedora](https://hub.docker.com/r/mullholland/fedora/)|38, 39|
|[Ubuntu](https://hub.docker.com/r/mullholland/ubuntu)|all|
|[Debian](https://hub.docker.com/r/mullholland/debian)|all|

The minimum version of Ansible required is 2.10, tests have been done to:

- The previous version.
- The current version.
- The development version.

If you find issues, please register them in [GitHub](https://github.com/mullholland/ansible-role-alertmanager_docker/issues).

## [License](#license)

[MIT](https://github.com/mullholland/ansible-role-alertmanager_docker/blob/master/LICENSE).

## [Author Information](#author-information)

[mullholland](https://mullholland.net)
