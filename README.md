փոխիր այնպես որ ես ոչ թե օգտագործել եմ sqlliteայլ postgres# 🏔️ Alpine Ansible Infrastructure 

[![Alpine Linux](https://img.shields.io/badge/Alpine_Linux-3.22+-0D597F?style=for-the-badge&logo=alpine-linux&logoColor=white)](https://alpinelinux.org/)
[![Ansible](https://img.shields.io/badge/Ansible-2.9+-EE0000?style=for-the-badge&logo=ansible&logoColor=white)](https://ansible.com/)
[![LXC](https://img.shields.io/badge/LXC-Container-2496ED?style=for-the-badge&logo=linux-containers&logoColor=white)](https://linuxcontainers.org/)

> **⚡ Lightweight Infrastructure Automation for Alpine Linux Containers**
> 
> A production-ready Ansible playbook specifically designed for Alpine Linux LXC containers, featuring optimized web server and database deployments.

---

## 🎯 Target Platforms

This playbook is **exclusively optimized** for:

- 🐧 **Alpine Linux 3.22+** (LXC Containers)
- 📦 **Proxmox LXC** environments
- 🏗️ **Linux Containers** (LXC/LXD)
- ☁️ **Minimal Container Distributions**

> ⚠️ **Note:** This playbook uses `apk` package manager and Alpine-specific configurations. It will **NOT** work on Ubuntu, Debian, CentOS, or other distributions.

---

## 🚀 Quick Start

### Prerequisites

```bash
# Local machine requirements
sudo apt install ansible sshpass  # Ubuntu/Debian
# OR
brew install ansible               # macOS
```

### 1. Clone & Configure

```bash
git clone <your-repo>
cd ansible-alpine-infrastructure

# Configure your container IPs
cp inventory.ini.example inventory.ini
# Edit inventory.ini with your Alpine container IPs
```

### 2. SSH Setup

```bash
# Generate SSH key (if needed)
ssh-keygen -t rsa -f ~/alpine-key

# Copy to your Alpine containers
ssh-copy-id -i ~/alpine-key.pub root@your-container-ip
```

### 3. Deploy Infrastructure

```bash
# Test connectivity
ansible all -i inventory.ini -m ping

# Deploy full stack
ansible-playbook -i inventory.ini site.yml
```

---

## 🏗️ Architecture

```
┌─────────────────┐    ┌─────────────────┐
│   Web Server    │    │   Database      │
│                 │    │                 │
│  Alpine Linux   │    │  Alpine Linux   │
│  + Nginx        │    │  + SQLite       │
│  Port: 80       │    │  Lightweight    │
└─────────────────┘    └─────────────────┘
```

## 📦 What Gets Deployed

### Web Server Container
- ✅ **Nginx** - Lightning-fast web server
- ✅ **Custom HTML** - Responsive landing page
- ✅ **SSL Ready** - Easy HTTPS configuration
- ✅ **Alpine Optimized** - Minimal resource usage

### Database Container
- ✅ **SQLite** - Zero-configuration database
- ✅ **Lightweight** - Perfect for containers
- ✅ **Backup Ready** - File-based storage
- ✅ **Python Support** - Full ORM compatibility

---

## 🔧 Configuration

### Inventory Setup

```ini
[web]
192.168.1.100 ansible_user=root ansible_ssh_private_key_file=~/alpine-key

[db]
192.168.1.101 ansible_user=root ansible_ssh_private_key_file=~/alpine-key
```

### Variables (`vars.yml`)

```yaml
# Web Configuration
server_name: your-domain.com
document_root: /var/www/html

# Database Configuration  
db_name: myapp_db
db_user: app_user
db_password: secure_password_123
```

---

## 🎮 Usage Examples

### Deploy Everything
```bash
ansible-playbook -i inventory.ini site.yml -v
```

### Web Server Only
```bash
ansible-playbook -i inventory.ini site.yml --limit web
```

### Database Only
```bash
ansible-playbook -i inventory.ini site.yml --limit db
```

### Dry Run (Check Mode)
```bash
ansible-playbook -i inventory.ini site.yml --check
```

---

## 🧪 Testing & Verification

### Automated Testing
```bash
# Test web server
curl http://your-web-server-ip

# Test database
ssh root@your-db-server "sqlite3 /var/lib/sqlite/myapp_db.db '.tables'"

# Check services
ansible all -i inventory.ini -m service -a "name=nginx state=started"
```

### Health Check Script
```bash
#!/bin/bash
echo "🏥 Health Check Starting..."
curl -f http://192.168.1.100 && echo "✅ Web Server OK" || echo "❌ Web Server Failed"
ssh root@192.168.1.101 'test -f /var/lib/sqlite/myapp_db.db' && echo "✅ Database OK" || echo "❌ Database Failed"
```

---

## 🛠️ Troubleshooting

### Common Issues

**🐛 Python Not Found**
```bash
# Install Python on Alpine containers
ansible all -i inventory.ini -m raw -a "apk add python3"
```

**🐛 SSH Permission Denied**
```bash
# Check SSH key permissions
chmod 600 ~/alpine-key
ssh -i ~/alpine-key root@container-ip
```

**🐛 Service Won't Start**
```bash
# Debug on container
ssh root@container-ip
rc-service nginx status
tail -f /var/log/nginx/error.log
```

---

## 📋 Directory Structure

```
ansible-alpine-infrastructure/
├── 📁 roles/
│   ├── 📁 web/
│   │   ├── 📁 tasks/        # Nginx installation & config
│   │   ├── 📁 templates/    # Nginx configuration templates
│   │   ├── 📁 files/        # Static files (HTML, assets)
│   │   └── 📁 handlers/     # Service restart handlers
│   └── 📁 db/
│       └── 📁 tasks/        # Database setup & initialization
├── 📁 playbooks/
│   ├── web.yml             # Web server playbook
│   └── db.yml              # Database playbook
├── 📄 site.yml             # Main orchestration playbook
├── 📄 inventory.ini        # Container inventory
├── 📄 vars.yml             # Configuration variables
├── 📄 ansible.cfg          # Ansible settings
└── 📄 README.md            # This file
```

---

## 🚦 Requirements

### Container Requirements
- Alpine Linux 3.22+
- Root access or sudo privileges
- SSH server running
- Minimum 512MB RAM
- 2GB disk space

### Local Requirements
- Ansible 2.9+
- Python 3.6+
- SSH client

---

## 🎨 Customization

### Adding Custom Pages
```bash
# Add files to roles/web/files/
echo "<h1>Custom Page</h1>" > roles/web/files/custom.html
```

### Custom Nginx Configuration
```bash
# Edit roles/web/templates/nginx.conf.j2
# Add your custom server blocks, SSL, etc.
```

### Database Schema
```bash
# Add initialization scripts to roles/db/files/
# They'll be executed during deployment
```

---

## 🚀 Performance Features

- ⚡ **Alpine Linux** - 5MB base image
- 🔥 **Nginx** - 10,000+ concurrent connections  
- 💾 **SQLite** - 35% faster than client/server DBs
- 🐳 **Container Optimized** - Minimal resource usage
- 📦 **Package Cached** - Faster subsequent deployments

---

## 🔐 Security Features

- 🔒 SSH key-based authentication
- 🛡️ Minimal attack surface (Alpine)  
- 🚪 Service isolation per container
- 📝 Configurable firewall rules
- 🔄 Automated security updates

---

## 🤝 Contributing

1. Fork the repository
2. Create your feature branch (`git checkout -b feature/alpine-optimization`)
3. Test on Alpine containers
4. Commit your changes (`git commit -am 'Add Alpine-specific feature'`)
5. Push to the branch (`git push origin feature/alpine-optimization`)
6. Create a Pull Request

---

## 📄 License

MIT License - see [LICENSE](LICENSE) file for details.

---

## 🙏 Acknowledgments

- 🏔️ [Alpine Linux Project](https://alpinelinux.org/) - The secure, lightweight base
- 📦 [Proxmox](https://proxmox.com/) - Excellent LXC container platform  
- 🤖 [Ansible Community](https://ansible.com/) - Automation made simple
- 🐧 [Linux Containers Project](https://linuxcontainers.org/) - Container innovation

---

<div align="center">

**Built with ❤️ for Alpine Linux containers**

[⭐ Star this repo](../../stargazers) • [🐛 Report Bug](../../issues) • [✨ Request Feature](../../issues)

</div>