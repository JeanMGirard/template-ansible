# template-ansible

Template for Ansible playbook repositories.

## Repository structure

```
.
├── ansible.cfg                  # Ansible configuration
├── requirements.yml             # Galaxy roles/collections
├── site.yml                     # Main playbook entry point
├── inventory/
│   ├── production               # Production inventory
│   ├── staging                  # Staging inventory
│   ├── group_vars/
│   │   ├── all/common.yml       # Variables for every host
│   │   ├── webservers/main.yml  # Variables for webservers group
│   │   └── dbservers/main.yml   # Variables for dbservers group
│   └── host_vars/
│       └── host1.example.com/   # Per-host variables
└── roles/
    ├── common/                  # Base configuration for all hosts
    ├── webserver/               # Nginx web server role
    └── database/                # PostgreSQL database role
```

Each role follows the standard Ansible role layout:

```
roles/<role>/
├── defaults/main.yml   # Default variable values
├── handlers/main.yml   # Handlers (e.g. service restarts)
├── meta/main.yml       # Role metadata and dependencies
├── tasks/main.yml      # Task list
└── templates/          # Jinja2 templates
```

## Prerequisites

- Python ≥ 3.9
- Ansible ≥ 2.14

Install Ansible:

```bash
pip install ansible
```

## Setup

1. **Clone / use this template** to create your repository.

2. **Install Galaxy dependencies** (if any are listed in `requirements.yml`):

   ```bash
   ansible-galaxy install -r requirements.yml
   ```

3. **Populate your inventory** – edit `inventory/production` and
   `inventory/staging` with your actual hostnames and IP addresses.

4. **Adjust variables** in `inventory/group_vars/` and
   `inventory/host_vars/` to match your environment.

## Usage

Run the full site playbook against production:

```bash
ansible-playbook site.yml -i inventory/production
```

Run against staging:

```bash
ansible-playbook site.yml -i inventory/staging
```

Limit execution to a specific group or host:

```bash
ansible-playbook site.yml -i inventory/production --limit webservers
ansible-playbook site.yml -i inventory/production --limit web1.example.com
```

Dry-run (check mode):

```bash
ansible-playbook site.yml -i inventory/production --check
```

## Adding a new role

```bash
ansible-galaxy role init roles/<role_name>
```

Then reference the role in `site.yml` and add it to the appropriate play.

## License

MIT
