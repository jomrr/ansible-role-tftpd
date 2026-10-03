# Ansible Role: tftpd

![GitHub](https://img.shields.io/github/license/jomrr/ansible-role-tftpd)
![GitHub last commit](https://img.shields.io/github/last-commit/jomrr/ansible-role-tftpd)
![GitHub issues](https://img.shields.io/github/issues-raw/jomrr/ansible-role-tftpd)
[![dev](https://img.shields.io/github/actions/workflow/status/jomrr/ansible-role-tftpd/dev.yml?branch=dev&label=dev)](https://github.com/jomrr/ansible-role-tftpd/actions/workflows/dev.yml?query=branch%3Adev)
[![main](https://img.shields.io/github/actions/workflow/status/jomrr/ansible-role-tftpd/main.yml?branch=main&label=main)](https://github.com/jomrr/ansible-role-tftpd/actions/workflows/main.yml?query=branch%3Amain)

Install and configure the tftp-hpa server.

## Purpose

Install tftp-hpa, configure its file root and keep the service available on UDP
port 69. Unchanged runs are idempotent. Every run restores a stopped or disabled
service or activation socket.

## Scope

### Managed

- Distribution packages for tftp-hpa and SELinux management where required.
- Ownership and permissions of the TFTP root directory.
- Distribution-native server configuration and service activation.
- Persistent SELinux file contexts and relabeling of the configured root and its
  contents.

### Not Managed

- Boot images, firmware, DHCP/PXE configuration and firewall rules.
- File ownership and Unix permissions below the TFTP root.

## Requirements

- A systemd host on one of the supported distributions.

## Dependencies

```yaml
collections:
  - name: community.general
    version: '>=12.0.0'
  - name: ansible.posix
    version: '>=2.0.0'
```

## Role Variables

### `tftpd_root`

Type: `path`. Required: `false`.

Absolute directory served by tftp-hpa; the filesystem root is not permitted.

Default:

```yaml
tftpd_root: /srv/tftp
```

### `tftpd_root_owner`

Type: `str`. Required: `false`.

Owner of the TFTP root directory.

Default:

```yaml
tftpd_root_owner: root
```

### `tftpd_root_group`

Type: `str`. Required: `false`.

Group of the TFTP root directory.

Default:

```yaml
tftpd_root_group: root
```

### `tftpd_root_mode`

Type: `str`. Required: `false`.

Quoted permission mode of the TFTP root directory.

Default:

```yaml
tftpd_root_mode: '0755'
```

### `tftpd_options`

Type: `str`. Required: `false`.

Command-line options for in.tftpd, in addition to the platform's service
arguments.
The default confines requests to tftpd_root; openSUSE also supplies its own
secure flag.

Default:

```yaml
tftpd_options: --secure
```

### `tftpd_setype`

Type: `str`. Required: `false`.

Persistent SELinux file type for the TFTP root and its contents when SELinux is
enabled.

Default:

```yaml
tftpd_setype: public_content_t
```

## Managed Files

- `/etc/default/tftpd-hpa` Debian and Ubuntu server configuration, owned by root
  with mode 0644.
- `/etc/systemd/system/tftp.service.d/override.conf` AlmaLinux and Fedora
  command override; the package-owned service unit remains intact.
- `/etc/sysconfig/tftp` openSUSE server configuration, owned by root with mode
  0644.

## Check Mode

Package, file, configuration and service tasks use their modules' check-mode
support.

- A check before initial installation can fail because the service or its
  configuration directory is not installed yet.
- Recursive SELinux relabeling runs only during normal convergence.

## Service Behavior

Debian and Ubuntu run tftpd-hpa.service. AlmaLinux, Fedora and openSUSE enable
and start tftp.socket, which activates tftp.service on demand. Configuration
changes restart the daemon after any required systemd reload.

## Security Notes

- TFTP has no authentication or encryption. Only publish files intended for all
  clients allowed by the network firewall. The default root is owned by root
  with mode 0755, and --secure confines requests to that directory.
- The default SELinux type public_content_t permits read access. Enabling
  uploads also requires appropriate daemon options, Unix file permissions and
  SELinux policy; these are not enabled by default.

## Operational Notes

- Configuration files are backed up before replacement. Obsolete TFTP roots and
  their content are not removed.
- tftp-hpa has no configuration-test mode. Debian's shell configuration is
  syntax-checked before installation; daemon option errors are reported when the
  service starts.

## Supported Platforms

| OS Family | Distribution | Version | Container Image |
| --------- | ------------ | ------- | --------------- |
| RedHat | AlmaLinux | latest | [jomrr/molecule-almalinux:latest](https://hub.docker.com/r/jomrr/molecule-almalinux) |
| Debian | Debian | latest | [jomrr/molecule-debian:latest](https://hub.docker.com/r/jomrr/molecule-debian) |
| RedHat | Fedora | latest | [jomrr/molecule-fedora:latest](https://hub.docker.com/r/jomrr/molecule-fedora) |
| Suse | OpenSuse Leap | latest | [jomrr/molecule-opensuse-leap:latest](https://hub.docker.com/r/jomrr/molecule-opensuse-leap) |
| Suse | OpenSuse Tumbleweed | latest | [jomrr/molecule-opensuse-tumbleweed:latest](https://hub.docker.com/r/jomrr/molecule-opensuse-tumbleweed) |
| Debian | Ubuntu | latest | [jomrr/molecule-ubuntu:latest](https://hub.docker.com/r/jomrr/molecule-ubuntu) |

## Example Playbook

### Simple example playbook

Minimal example for applying this role.

```yaml
---
- name: "Configure tftpd"
  hosts: "tftpd"
  gather_facts: true
  roles:
    - role: "jomrr.tftpd"
```

## References

- [in.tftpd manual](https://manpages.debian.org/trixie/tftpd-hpa/in.tftpd.8.en.html)

## Author

[Jonas Mauer](https://github.com/jomrr)

## License

This project is licensed under the MIT License.
See [LICENSE](LICENSE) for the full license text.

Copyright (c) 2021 Jonas Mauer.
