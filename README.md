# Dependency Management & Facts Usage

This project uses a `collections.yml` and `requirements.yml` to manage Ansible Galaxy collections. Always install dependencies before running playbooks or tests:

```sh
ansible-galaxy collection install -r collections.yml
# or
ansible-galaxy collection install -r requirements.yml
```

# OpenNIC Setup with Ansible

This Ansible playbook automates the setup of an OpenNIC Tier 2 DNS resolver on various Linux distributions. It supports multiple DNS server implementations and provides production-ready configurations.

## Supported Distributions

- Debian
- Ubuntu
- RHEL-based (Red Hat Enterprise Linux, Rocky Linux, AlmaLinux, Oracle Linux)
- Fedora

## Supported DNS Servers

- **BIND** (default) - ISC BIND DNS server, production-ready
- **djbdns** - D.J. Bernstein's DNS server suite, lightweight
- **Trust-DNS** - Modern Rust-based DNS server, experimental/not recommended for production

## Features

### Core Functionality
- Installs and configures DNS server
- Sets up OpenNIC zone slaves for alternative TLDs
- Configures firewall rules for DNS (port 53)
- Enables production-ready OS adjustments (NTP, logging, etc.)

### Modern DNS Features (configurable)
- DNSSEC validation
- DNS-over-HTTPS (DoH) - not implemented
- DNS-over-TLS (DoT) - supported for BIND and Trust-DNS
- DNSSEC signing - planned
- Rate limiting - supported for BIND
- Query logging - supported for BIND

### Maintenance
- Zone file updates
- Service restarts
- Health checks
- Log rotation
- Config backups
- OS updates

## Feature Matrix

| Feature | BIND | djbdns | CoreDNS | Trust-DNS |
|---------|------|--------|----------|-----------|
| Basic Setup | ✅ | ✅ (Debian) | ✅ | ⚠️ (Experimental) |
| DNSSEC Validation | ✅ | ❌ | ✅ | ❌ |
| DoH | ❌ | ❌ | ❌ | ❌ |
| DoT | ✅ | ❌ | ✅ | ✅ |
| Rate Limiting | ✅ | ✅ | ✅ | ❌ |
| Query Logging | ✅ | ❌ | ✅ | ❌ |
| Minimal Logging | ✅ | ✅ | ✅ | ✅ |
| Maintenance Tasks | ✅ | ✅ | ✅ | ✅ |

## Quick Start

1. Clone this repository
2. Update your `/etc/ansible/hosts` or create a custom inventory:
   ```
   [opennic]
   server1.example.com
   server2.example.com
   ```
3. Run the playbook:
   ```
   ansible-playbook site.yml
   ```

## Configuration

### Variables

Set variables in your playbook or inventory:

```yaml
# Choose DNS server
dns_server: bind  # Options: bind, djbdns, trust_dns

# DNSSEC settings
bind_dnssec_validation: auto

# Enable features
bind_enable_doh: false
bind_enable_dot: false
bind_enable_dnssec_signing: false
bind_enable_rate_limiting: false
bind_enable_query_logging: false

# OpenNIC master servers (update as needed)
opennic_masters:
  - 161.97.219.84
  - 2001:470:4212:10:0:100:53:10
  # ... more IPs
```

### Advanced Features

Set these variables to enable advanced DNS features (BIND only):

```yaml
# DNS-over-TLS (DoT)
bind_enable_dot: true
bind_dot_port: 853
bind_dot_certificate: "/etc/ssl/certs/ssl-cert-snakeoil.pem"
bind_dot_key: "/etc/ssl/private/ssl-cert-snakeoil.key"

# Rate limiting
bind_enable_rate_limiting: true
bind_rate_limit_requests: 10  # requests per second
bind_rate_limit_window: 5     # seconds

# Logging
bind_enable_query_logging: true
bind_log_file: "/var/log/named/queries.log"

# Minimal/no logging (for privacy/legal requirements)
bind_minimal_logging: true  # Disables all logging except errors
```

## Maintenance

Run maintenance tasks:

```bash
ansible-playbook site.yml --tags maintenance
```

## Development

### Adding New Distributions

1. Create a new role: `ansible-galaxy role init roles/newdistro`
2. Implement OS-specific setup in `roles/newdistro/tasks/main.yml`
3. Update `site.yml` with the new role condition
4. Test on the target distribution

### Adding New DNS Servers

1. Create DNS server role: `ansible-galaxy role init roles/newdns`
2. Implement installation and configuration
3. Add feature support as needed
4. Update feature matrix in README
5. Add to `dns_server` options

## Requirements

- Ansible 2.9+
- Target servers with SSH access
- Python on target servers (for Ansible)

## Contributing

1. Fork the repository
2. Create a feature branch
3. Make your changes
4. Test thoroughly
5. Submit a pull request

## License

Apache 2.0

## Disclaimer

This playbook sets up a public DNS resolver. Ensure proper security measures are in place, especially for internet-facing servers.
