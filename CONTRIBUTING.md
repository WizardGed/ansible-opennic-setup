# Contributing to Ansible OpenNIC Setup

Thank you for your interest in contributing! This document outlines the process for contributing to this project.

## Development Setup

1. Fork the repository
2. Clone your fork: `git clone https://github.com/yourusername/ansible-opennic-setup.git`
3. Create a feature branch: `git checkout -b feature/your-feature`
4. Make your changes
5. Test your changes
6. Submit a pull request

## Code Style

- Follow Ansible best practices
- Use descriptive variable names
- Include comments for complex logic
- Test on multiple distributions when possible

## Testing

- Test playbooks on target distributions
- Verify DNS resolution works
- Check firewall rules are applied correctly
- Ensure services start properly

## Pull Request Process

1. Update the README.md if needed
2. Update feature matrix for new features
3. Ensure all tests pass
4. Provide a clear description of changes
5. Reference any related issues

## Adding New Features

### New Distribution Support
1. Create a new role in `roles/`
2. Implement OS-specific tasks
3. Update `site.yml` with new role
4. Test on the distribution
5. Update README feature matrix

### New DNS Server Support
1. Create DNS server role
2. Implement installation and configuration
3. Add variables for customization
4. Update maintenance tasks if needed
5. Update README

## Issues

- Use GitHub issues for bug reports and feature requests
- Provide detailed information including:
  - Distribution and version
  - DNS server used
  - Error messages
  - Steps to reproduce