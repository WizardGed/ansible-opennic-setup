# TODO: OpenNIC Root Server Automation and Integration

## Automate OpenNIC Tier 1 Root Server List

- [ ] Create script to scrape OpenNIC Tier 1 root server IPs from https://servers.opennic.org/?tier=1
    - Parse IPv4 and IPv6 addresses from the HTML
    - Output as a list suitable for Ansible variables (bind_opennic_masters, opennic_masters)
- [ ] Integrate dynamic root server list into bind and djbdns roles
    - Use the script output to update the defaults or vars for both roles
    - Ensure both roles use the same authoritative list
- [ ] Add CLI tool or playbook to update root server list on demand
    - Provide a command or playbook to fetch and update the list
    - Document usage for maintainers and CI

## Notes
- Provide a static fallback list for offline/CI use
- Consider contributing improvements upstream or to OpenNIC tools if useful
