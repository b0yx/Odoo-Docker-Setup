Production-ready Odoo 17 + PostgreSQL 16 Docker environment.

---

## 🏗 Architecture

- **Odoo 17**
    
- **PostgreSQL 16**
    
- Docker Compose (v2+)
    
- Docker secrets for DB password
    
- Persistent volumes
    

---

## 📂 Project Structure

Odoo-docker-directory/  
├── addons/                # Custom addons  
├── config/  
│   └── odoo.conf          # Odoo configuration  
├── docker-compose.yaml  
├── odoo_pg_pass           # Database password file  
└── sessions/              # (optional) session storage

---

## ⚙ Requirements

- Docker 24+
    
- Docker Compose v2+
    
- 4GB RAM minimum recommended
    

Verify installation:

docker --version  
docker compose version

---

## 🔐 Configure Database Password

Create a file:

odoo_pg_pass

Inside it put **only the password**, no spaces:

odoo

This will be injected securely using Docker secrets.

---

## 📝 Odoo Configuration

File: `config/odoo.conf`

[options]  
addons_path = /mnt/extra-addons  
data_dir = /var/lib/odoo  
  
admin_passwd = admin_master_password  
  
db_host = db  
db_port = 5432  
db_user = odoo  
db_password = odoo

Replace `admin_master_password` with a strong password.

---

## 🐳 Start the Environment

docker compose up -d

Check running containers:

docker compose ps

---

## 🌐 Access Odoo

Open:

http://localhost:8069

---

## 📋 Useful Commands

### View logs

docker compose logs -f

### Restart services

docker compose restart

### Stop everything

docker compose down

### Remove volumes (⚠ deletes database)

docker compose down -v

---

## 🗄 Access PostgreSQL Manually

docker compose exec db psql -U odoo -d postgres

List databases:

\l

---

## 📦 Install Custom Addons

Place modules inside:

addons/

Then restart:

docker compose restart

---

## 🔒 Security Notes (Important for Production)

- Do NOT expose PostgreSQL port publicly
    
- Use strong `admin_passwd`
    
- Use reverse proxy (Nginx + SSL)
    
- Enable `proxy_mode = True` if behind reverse proxy
    
- Do not use `odoo` as production password
    

---

## 🏢 Production Recommendation

For production environments:

- Add Nginx reverse proxy
    
- Use HTTPS (Let’s Encrypt)
    
- Enable automatic backups
    
- Configure workers in `odoo.conf`
    

Example worker config:

workers = 4  
limit_memory_hard = 2684354560  
limit_memory_soft = 2147483648  
limit_request = 8192  
limit_time_cpu = 600  
limit_time_real = 1200

---

