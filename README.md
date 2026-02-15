# 🚀 Ansible Notes App - Automated Deployment

Complete Ansible automation for deploying a Flask-based Notes application to multiple AWS EC2 instances running Amazon Linux 2023.

## 📋 What This Does

This Ansible playbook automates the complete deployment of a note-taking web application including:

- ✅ System packages installation (Python, Git, MariaDB, Firewall)
- ✅ MariaDB database setup and user creation
- ✅ Application deployment from GitHub
- ✅ Python virtual environment and dependencies
- ✅ Systemd service configuration (auto-start on boot)
- ✅ Firewall configuration
- ✅ Automated daily database backups

## 🎯 Deployment Results

**Deployment Time:** ~3-5 minutes for multiple servers  
**Manual Time Saved:** 30-45 minutes per server

Successfully deployed to **3 EC2 instances** simultaneously.

## 📁 Project Structure
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

## 🚀 Quick Start

### Prerequisites

- Ansible 2.9+
- AWS EC2 instances (Amazon Linux 2023)
- SSH access to target servers

### Deploy
```bash
# Test connection
ansible all -m ping

# Deploy to all servers
ansible-playbook playbook.yml

# Deploy to specific tags
ansible-playbook playbook.yml --tags "database,deploy"
```

## 🔧 Configuration

Edit `group_vars/all.yml` to customize:
```yaml
app_port: 8000
gunicorn_workers: 4
backup_retention_days: 7
```

## 🔒 Security

- Passwords stored in encrypted Ansible Vault
- Firewall configured automatically
- Database user with minimal privileges

## 📊 Monitoring
```bash
# Check service status
ansible all -m shell -a "systemctl status notes-app" -b

# View logs
ansible all -m shell -a "journalctl -u notes-app -n 50" -b

# Check backups
ansible all -m shell -a "ls -lh /backup/" -b
```

## ✅ Tested Features

- [x] Multi-server deployment
- [x] Auto-start on boot
- [x] Automated backups
- [x] Database operations (CRUD)
- [x] Service restart
- [x] Firewall configuration

## 👤 Author

**Your Name**

## 📄 License

MIT License

---

**Automated with Ansible** ⚡

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

- Never commit `.vault_pass` or `group_vars/vault.yml` unencrypted
- Never commit SSH private keys
- Change all default passwords
- The vault file in this repo is encrypted - you need to create your own

