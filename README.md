# ansible-role-tftpd

![GitHub](https://img.shields.io/github/license/jam82/ansible-role-tftpd) ![GitHub last commit](https://img.shields.io/github/last-commit/jam82/ansible-role-tftpd) ![GitHub issues](https://img.shields.io/github/issues-raw/jam82/ansible-role-tftpd) ![Travis (.com) branch](https://img.shields.io/travis/com/jam82/ansible-role-tftpd/main?label=travis) [![Molecule](https://github.com/jam82/ansible-role-tftpd/actions/workflows/molecule.yml/badge.svg)](https://github.com/jam82/ansible-role-tftpd/actions/workflows/molecule.yml)

**Ansible role for setting up tftpd-hpa.**

This role does not manage your firewall.

## Supported Platforms

| OS Family | Distribution  | Latest | Supported Version(s) | Comment |
|-----------|---------------|--------|----------------------|---------|
| Alpine    | Alpine        | :heavy_check_mark: | 3.12, 3.13 | |
| Archlinux | Archlinux     | :heavy_check_mark: | - | |
|           | Manjaro       | :heavy_check_mark: | - | |
| Debian    | Debian        | :heavy_check_mark: | 10, 11 | |
|           | Ubuntu        | :heavy_check_mark: | 18.04, 20.04 | |
| RedHat    | Almalinux     | :heavy_check_mark: | 8 | |
|           | Amazonlinux   | :x: | - | not tested, image not working |
|           | Centos        | :heavy_check_mark: | 8 | |
|           | Fedora        | :heavy_check_mark: | 33, 34, Rawhide | |
|           | Oraclelinux   | :heavy_check_mark: | 8 | |
| Suse      | OpenSuse Leap | :heavy_check_mark: | 15.1, 15.2, 15.3 | |
|           | Tumbleweed    | :heavy_check_mark: | - | |

## Requirements

Ansible 2.9 or higher.

## Variables

Variables and defaults for this role.

### defaults/main.yml

```yaml
---
# role: ansible-role-tftpd
# file: defaults/main.yml

# The role is disabled by default, so you do not get in trouble.
# Checked in tasks/main.yml which includes tasks/tasks.yml if enabled.
tftpd_role_enabled: false

# the tftpd-hpa root directory. we are respecting FHS here...
# and do not use /tftpboot or other strange stuff.
tftpd_root: /srv/tftp

# owner of the tftpd root directory
tftpd_root_owner: "root"

# group of the tftpd root directory
tftpd_root_group: "root"

# access mode of the tftpd root directory
tftpd_root_mode: 0755

# tftpd-hpa server flags to set
# Alpine adds "-R 4096:32767" by default.
# see https://linux.die.net/man/8/in.tftpd for more options
tftpd_options: "--secure"

# selinux type
# tftpdir_rw_t for tftp only, see https://linux.die.net/man/8/tftpd_selinux
tftpd_setype: public_content_t
```

## Dependencies

None.

## Example Playbook

```yaml
---
# role: ansible-role-tftpd
# play: tftpd
# file: tftpd.yml

- hosts: all
  become: true
  gather_facts: true
  vars:
    tftpd_role_enabled: true
  roles:
    - role: ansible-role-tftpd
```

## License and Author

- Author:: [jam82](https://github.com/jam82/)
- Copyright:: 2021, [jam82](https://github.com/jam82/)

Licensed under [MIT License](https://opensource.org/licenses/MIT).
See [LICENSE](https://github.com/jam82/ansible-role-tftpd/blob/master/LICENSE) file in repository.

## References

- [TFTP - Archwiki](https://wiki.archlinux.org/title/TFTP)
- [Debian man pages](https://manpages.debian.org/testing/tftpd-hpa/tftpd.8.en.html)
- [man in.tftpd(8)](https://linux.die.net/man/8/in.tftpd)
- [man tftpd_selinux(8)](https://linux.die.net/man/8/tftpd_selinux)
