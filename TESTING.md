# Dependency Management

Before running Molecule or Ansible tests, install required collections:

```sh
ansible-galaxy collection install -r collections.yml
# or
ansible-galaxy collection install -r requirements.yml
```

# Local and CI Testing Instructions

## Local Molecule Testing

To run molecule tests locally and ensure the `bind` role is found, set the `ANSIBLE_ROLES_PATH` environment variable to your project's `roles` directory:

```sh
export ANSIBLE_ROLES_PATH="$PWD/roles"
cd roles/bind
molecule test
```

Or, from the project root for the new structure:

```sh
export ANSIBLE_ROLES_PATH="$PWD/roles"
molecule test
```

## GitHub Actions CI

The workflow sets `ANSIBLE_ROLES_PATH` automatically before running molecule tests:

```yaml
- name: Run Molecule tests
  run: |
    export ANSIBLE_ROLES_PATH="$GITHUB_WORKSPACE/roles"
    cd roles/${{ matrix.role }}
    molecule test
```

This ensures both local and CI runs can always find the correct roles.
