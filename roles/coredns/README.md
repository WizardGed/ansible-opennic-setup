# Ansible Role: CoreDNS with OpenNIC

This role installs and configures [CoreDNS](https://coredns.io/) to use OpenNIC root servers as forwarders. It is designed to be robust, portable, and CI-friendly, following the same best practices as the bind and djbdns roles in this repository.

## Features
- Installs CoreDNS (binary download for CI/Molecule)
- Configures `/etc/coredns/Corefile` to forward DNS queries to OpenNIC root servers
- Optionally configures `/etc/resolv.conf` to use OpenNIC resolvers
- Supports Debian and RedHat families
- Molecule scenario for CI testing

## Variables
- `coredns_opennic_masters`: List of OpenNIC root server IPs (default: see `defaults/main.yml`)

## Example Playbook
```yaml
- hosts: all
  become: true
  roles:
    - role: coredns
```

## Molecule Testing
Run the following to test the role in CI:
```sh
molecule test -s coredns
```

## License
Apache-2.0
