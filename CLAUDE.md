# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project Overview

Ansible project for provisioning Ubuntu/Debian servers to run a Strapi CMS application with PostgreSQL, Nginx, and SSL via Let's Encrypt.

## Commands

```bash
# Install dependencies (requires vault password)
ansible-playbook dependencies.yml --ask-vault-pass

# Deploy/update Strapi from Codeberg
ansible-playbook deploy.yml

# Run with verbose output
ansible-playbook -v dependencies.yml --ask-vault-pass

# Check syntax without executing
ansible-playbook --syntax-check dependencies.yml

# Dry run (check mode)
ansible-playbook --check dependencies.yml --ask-vault-pass

# Edit encrypted secrets
ansible-vault edit vars/secrets.yml
```

## Architecture

Two-playbook structure:
- **dependencies.yml** - System provisioning: Node.js, PostgreSQL, Nginx, PM2, SSL, and strapi user/group creation
- **deploy.yml** - Application deployment: clones repo, installs npm deps, builds, manages PM2 process

Key configuration:
- Secrets stored in `vars/secrets.yml` (vault-encrypted)
- Dedicated `strapi` user runs the application
- PM2 manages the Strapi process with systemd integration
- Nginx reverse proxy at `txcms.dicefriendly.com`
- App directory: `/opt/strapi`

The inventory group `myhosts` defines target servers. SSH agent forwarding enabled for git authentication.
