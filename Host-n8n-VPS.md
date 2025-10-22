# Install and Configure n8n

n8n is an open-source workflow automation tool that allows you to connect APIs, services, and databases to automate repetitive tasks. It runs efficiently alongside existing applications on your VPS using Docker and Nginx.

### Install Docker and Docker Compose
```shell
apt update && apt install -y docker.io docker-compose
systemctl enable docker
systemctl start docker
```

### Create n8n directory
```shell
mkdir -p /opt/n8n
cd /opt/n8n
```

### Create Docker Compose file
```shell
nano /opt/n8n/docker-compose.yml
```

### Paste the following configuration:
```shell
version: '3.8'

services:
  n8n:
    image: n8nio/n8n
    container_name: n8n
    restart: always
    ports:
      - "127.0.0.1:5678:5678"
    environment:
      - N8N_BASIC_AUTH_ACTIVE=true
      - N8N_BASIC_AUTH_USER=admin
      - N8N_BASIC_AUTH_PASSWORD=ChangeThisStrongPassword
      - N8N_HOST=n8n.yourdomain.com
      - N8N_PORT=5678
      - N8N_PROTOCOL=https
      - WEBHOOK_URL=https://n8n.yourdomain.com/
      - GENERIC_TIMEZONE=Asia/Dhaka
    volumes:
      - ./n8n_data:/home/node/.n8n

```

### Fix permission for data directory
```shell
mkdir -p /opt/n8n/n8n_data
chown -R 1000:1000 /opt/n8n/n8n_data
chmod -R 755 /opt/n8n/n8n_data
```

### Start n8n
```shell
cd /opt/n8n
docker-compose up -d
```

## Configure Nginx for n8n
Now configure the Nginx for n8n. Please note, you need to complete Basic Instalation first.

### Create a new Nginx configuration file:
```shell
nano /etc/nginx/sites-available/n8n.yourdomain.com
```

### Paste the configuration below:
```shell
server {
    listen 80;
    server_name n8n.yourdomain.com;

    location / {
        proxy_pass http://127.0.0.1:5678;
        proxy_http_version 1.1;
        proxy_set_header Upgrade $http_upgrade;
        proxy_set_header Connection 'upgrade';
        proxy_set_header Host $host;
        proxy_cache_bypass $http_upgrade;
        proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
        proxy_set_header X-Forwarded-Proto $scheme;
    }
}
```

### Enable the configuration and reload Nginx:
```shell
ln -s /etc/nginx/sites-available/n8n.yourdomain.com /etc/nginx/sites-enabled/
nginx -t
systemctl reload nginx
```

### Secure n8n with SSL
Use Let’s Encrypt to enable HTTPS. It's completly free.

```shell
certbot --nginx -d n8n.yourdomain.com
```

If all goes fine, your n8n should now be live at:
👉 https://n8n.yourdomain.com

# About me
I’m a passionate web developer with experience in building scalable and secure applications. I specialize in Laravel, Django, JavaScript and server management with Nginx. I enjoy automating workflows, optimizing performance, and deploying modern web solutions on VPS environments. I believe in clean code, continuous learning, and sharing knowledge through open-source contributions.

## Connect & Support

[![LinkedIn](https://img.shields.io/badge/LinkedIn-Connect-blue?style=for-the-badge&logo=linkedin)](https://www.linkedin.com/in/firoz-ebna-jobaier)
[![Buy Me a Coffee](https://img.shields.io/badge/Buy_Me_a_Coffee-Support-yellow?style=for-the-badge&logo=buymeacoffee)](buymeacoffee.com/yenHunter)
[![Fork me on GitHub](https://img.shields.io/badge/Fork_on_GitHub-000?style=for-the-badge&logo=github)](https://github.com/yenHunter)
