# Ansible Notes App — Automated Flask Deployment on AWS

Production-ready Ansible role for automated deployment of Flask applications on AWS EC2 using Infrastructure as Code practices.

Deploy a full production Flask stack across multiple servers in under 5 minutes.

## Architecture Overview

This project automates deployment of a Flask application with:

* Ansible Control Node
* AWS EC2 instances
* Flask application
* MariaDB database
* Gunicorn
* Systemd service
* Automated backups
* Firewall configuration

Supports multi-server deployment.

## Why This Project Matters

Manual deployment is slow and error-prone.

This automation provides:

* Repeatable deployments
* Production-ready configuration
* Secure database setup
* Automated service management
* Scalable deployment across multiple servers

**Deployment time reduced from 30–45 minutes to about 3–5 minutes.**

## DevOps Practices Applied

* Infrastructure as Code
* Idempotent automation
* Configuration management with Ansible
* Secrets management using Ansible Vault
* Systemd service orchestration
* Automated backups
* Firewall security
* Service reliability and failure handling

## What This Automation Deploys

* Python and system dependencies
* Flask application from GitHub
* Python virtual environment
* Gunicorn
* MariaDB database and user
* Systemd service
* Automated daily backups
* Firewall rules

## Reusability

This role can deploy any Flask-based application by changing variables such as:

* Repository URL
* Database credentials
* Service name
* Application port

No code changes required.

## Project Structure

```
ansible-notes-app/
├── ansible.cfg                          # Ansible configuration
├── playbook.yml                         # Main playbook
├── group_vars/
│   ├── all.yml                         # Variables
│   └── vault.yml                       # Encrypted secrets
└── roles/
    └── notes-app/
        ├── tasks/
        │   ├── main.yml
        │   ├── install_dependencies.yml
        │   ├── setup_database.yml
        │   ├── deploy_application.yml
        │   ├── configure_service.yml
        │   └── setup_backup.yml
        ├── templates/
        │   ├── .env.j2
        │   ├── notes-app.service.j2
        │   └── backup-notes.sh.j2
        ├── handlers/
        │   └── main.yml
        ├── vars/
        │   └── main.yml
        └── defaults/
            └── main.yml
```

## Requirements

* Ansible 2.9+
* AWS EC2 instances (Amazon Linux 2023)
* SSH access

## Install as Ansible Role

```bash
ansible-galaxy install filopateer_shaker.notes_app
```

## Deployment

### Test connection:
```bash
ansible all -m ping
```

### Deploy application:
```bash
ansible-playbook playbook.yml
```

### Deploy specific components:
```bash
ansible-playbook playbook.yml --tags "database,deploy"
```

## Configuration

Edit variables in `group_vars/all.yml`

Example:
```yaml
app_port: 8000
gunicorn_workers: 4
backup_retention_days: 7
```

## Security

* Secrets stored in Ansible Vault
* Firewall configured automatically
* Database user with minimal privileges
* Environment variables managed securely via templates and vault

## Monitoring

### Check service:
```bash
ansible all -m shell -a "systemctl status notes-app" -b
```

### View logs:
```bash
ansible all -m shell -a "journalctl -u notes-app -n 50" -b
```

### Check backups:
```bash
ansible all -m shell -a "ls -lh /backup/" -b
```

## Troubleshooting

**Service not starting:**
```bash
journalctl -u notes-app
```

**Database connection issues:**
Verify vault credentials

**Port not accessible:**
Check firewall rules

## 🛠️ Setup Instructions for New Users

### 1. Clone the Repository
```bash
git clone https://github.com/Filopateer-Shaker/notes-app-ansible.git
cd notes-app-ansible
```

### 2. Create Vault Password File
```bash
echo "your-secure-password" > .vault_pass
chmod 600 .vault_pass
```

### 3. Create Encrypted Vault
```bash
ansible-vault create group_vars/vault.yml --vault-password-file .vault_pass
```

Add this content (change the passwords):
```yaml
---
vault_db_password: "YourSecureDBPassword"
vault_db_root_password: "YourSecureRootPassword"
vault_secret_key: "generate-with-command-below"
```

**Generate secret key:**
```bash
python3 -c "import secrets; print(secrets.token_hex(32))"
```

### 4. Create Inventory File

Create `inventory.yml`:
```yaml
---
all:
  children:
    web_servers:
      hosts:
        my-server:
          ansible_host: YOUR_EC2_PUBLIC_IP
          ansible_user: ec2-user
          ansible_ssh_private_key_file: ~/.ssh/your-key.pem
          ansible_python_interpreter: /usr/bin/python3
```

### 5. Update ansible.cfg

Edit `ansible.cfg` and change:
```ini
[defaults]
inventory = inventory.yml
```

### 6. Test Connection
```bash
ansible all -m ping
```

### 7. Deploy
```bash
ansible-playbook playbook.yml
```

## 🔐 Security Notes

* Never commit `.vault_pass` or `group_vars/vault.yml` unencrypted
* Never commit SSH private keys
* Change all default passwords
* The vault file in this repo is encrypted - you need to create your own

## ✅ Tested Features

* Multi-server deployment
* Auto-start on boot
* Automated backups
* Database operations (CRUD)
* Service restart
* Firewall configuration

## Impact

* Deployment time reduced by more than 90%
* Production-ready automation
* Scalable multi-server deployment
* Repeatable infrastructure setup

## Author

**Filopateer Shaker**  
DevOps Engineer | AWS | Ansible | Infrastructure as Code

## Future Improvements

* CI pipeline with GitHub Actions
* Molecule testing
* Terraform provisioning
* Dockerized deployment

---

**Automated with Ansible** ⚡
