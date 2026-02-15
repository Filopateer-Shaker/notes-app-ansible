# Ansible Role: Notes App

Automates deployment of a Flask-based Notes application with MariaDB on Amazon Linux 2023.

## Features

- ✅ Full stack deployment (Python, Flask, MariaDB, Gunicorn)
- ✅ Systemd service configuration (auto-start on boot)
- ✅ Automated daily database backups
- ✅ Firewall configuration
- ✅ Security hardening

## Requirements

- Ansible 2.9+
- Target: Amazon Linux 2023 or RHEL 10
- Python 3.8+

## Role Variables
```yaml
app_port: 8000
gunicorn_workers: 4
backup_enabled: true
backup_retention_days: 7
db_name: notesdb
db_user: notesuser
```

See `defaults/main.yml` for all variables.

## Dependencies

None.

## Example Playbook
```yaml
- hosts: servers
  become: true
  
  vars:
    vault_db_password: "SecurePassword"
    vault_db_root_password: "SecureRootPassword"
    vault_secret_key: "your-secret-key"
  
  roles:
    - filopateer_shaker.notes_app
```

## Installation
```bash
ansible-galaxy install filopateer_shaker.notes_app
```

## License

MIT

## Author

Filopateer Shaker
- GitHub: [@Filopateer-Shaker](https://github.com/Filopateer-Shaker)
