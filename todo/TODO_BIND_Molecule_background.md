# TODO: Enable BIND foreground/background testing in Molecule containers

Currently, the DNS resolution test (dig @localhost ...) is skipped in CI and container environments because systemd is not running and BIND is not started as a service.

## Suggestion for future implementation

To enable DNS testing in containers, consider starting BIND in the foreground as a background process before running dig:

```yaml
- name: Start BIND in background (Molecule/CI only)
  ansible.builtin.shell: |
    named -c {{ bind_main_config }} -g &
  args:
    creates: /var/run/named/named.pid
  async: 10
  poll: 0
  when:
    - ansible_service_mgr != 'systemd'
    - bind_main_config is defined
```

- Ensure the config path and permissions are correct for your image.
- Only use this for test environments, not production.

## Next steps
- Implement and test this approach in Molecule.
- Remove the skip condition for the dig test when this is reliable.
