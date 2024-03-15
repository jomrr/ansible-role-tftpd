# Ansible role tftpd

![GitHub](https://img.shields.io/github/license/jam82/ansible-role-tftpd) ![GitHub last commit](https://img.shields.io/github/last-commit/jam82/ansible-role-tftpd) ![GitHub issues](https://img.shields.io/github/issues-raw/jam82/ansible-role-tftpd)

**Ansible role for setting up tftpd.**

## Description

This Ansible role installs and configures tftpd on supported platforms.

## Prerequisites

This role has no special prerequisites.

### System packages (Fedora)

- `python3` (Python 3.8 or later)

### Python (requirements.txt)

- ansible >= 2.15

## Dependencies (requirements.yml)

This role has no dependencies.

## Supported Platforms

| OS Family | Distribution | Version | Container Image |
|-----------|--------------|---------|-----------------|
| RedHat | AlmaLinux | 8 | [jam82/molecule-almalinux:8]( https://hub.docker.com/r/jam82/molecule-almalinux ) |
| | | 9 | [jam82/molecule-almalinux:9]( https://hub.docker.com/r/jam82/molecule-almalinux ) |
| Alpine | Alpine | 3.18 | [jam82/molecule-alpine:3.18]( https://hub.docker.com/r/jam82/molecule-alpine ) |
| | | 3.19 | [jam82/molecule-alpine:3.19]( https://hub.docker.com/r/jam82/molecule-alpine ) |
| Debian | Debian | 11 | [jam82/molecule-debian:11]( https://hub.docker.com/r/jam82/molecule-debian ) |
| | | 12 | [jam82/molecule-debian:12]( https://hub.docker.com/r/jam82/molecule-debian ) |
| RedHat | Fedora | 39 | [jam82/molecule-fedora:39]( https://hub.docker.com/r/jam82/molecule-fedora ) |
| | | 40 | [jam82/molecule-fedora:40]( https://hub.docker.com/r/jam82/molecule-fedora ) |
| | | rawhide | [jam82/molecule-fedora:rawhide]( https://hub.docker.com/r/jam82/molecule-fedora ) |
| Debian | Ubuntu | 20.04 | [jam82/molecule-ubuntu:20.04]( https://hub.docker.com/r/jam82/molecule-ubuntu ) |
| | | 22.04 | [jam82/molecule-ubuntu:22.04]( https://hub.docker.com/r/jam82/molecule-ubuntu ) |
| | | 24.04 | [jam82/molecule-ubuntu:24.04]( https://hub.docker.com/r/jam82/molecule-ubuntu ) |

## Role Variables

No role default variables specified, see [defaults/main.yml](defaults/main.yml).

## Example Playbook

Example playbooks(s) that show how to use this role.

## Simple example playbook

A simple default example playbook for using jam82.tftpd.
```yaml
---
# name: "jam82.tftpd"
# file: "playbook_tftpd.yml"

- name: "PLAYBOOK | tftpd"
  hosts: all
  gather_facts: true
  roles:
    - role: "jam82.tftpd"
```

## Author(s) and License

- :octocat:                 Author::    [jam82](https://github.com/jam82)
- :triangular_flag_on_post: Copyright:: 2024, Jonas Mauer
- :page_with_curl:          License::   [MIT](LICENSE)


---
