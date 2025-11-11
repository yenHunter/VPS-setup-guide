# Host CodeIgniter Site

CodeIgniter is a lightweight PHP framework for building fast and dynamic web applications. Follow these steps to deploy your CodeIgniter project on your VPS using Nginx.

## Configure CodeIgniter application

Follow the steps below to configure your CodeIgniter application on the VPS.
Please complete [Basic-Installation](./Basic-Installation.md) if not completed.

### Create CodeIgniter project directory

```shell
mkdir -p /var/www/yourcodeignitersitefolder
cd /var/www/yourcodeignitersitefolder
```

### Clone CodeIgniter project from GitHub

```shell
git clone git@github.com:user_name/repo_name.git .
```

### Install dependencies (if using Composer)

If your CodeIgniter project uses Composer (CodeIgniter 4+):

```shell
composer install
```

### Configure environment variables

For **CodeIgniter 4**:

```shell
cp env .env
nano .env
```

For **CodeIgniter 3**, edit `application/config/config.php` and `application/config/database.php` as needed.

### Create MySQL database

Log into MySQL and create a new database with privileges:

```shell
mysql -u root -p
CREATE DATABASE yourcodeignitersitedb;
GRANT ALL PRIVILEGES ON yourcodeignitersitedb.* TO 'user_name'@'localhost';
FLUSH PRIVILEGES;
exit
```

### Set correct permissions

```shell
chown -R www-data:www-data /var/www/yourcodeignitersitefolder/*
chmod -R 775 /var/www/yourcodeignitersitefolder/writable
```
> For **CodeIgniter 3**, set permissions for the `application/cache` and `application/logs` folders instead of `writable`.

---

## Configure Nginx for CodeIgniter

### Create a new site configuration file:

```shell
nano /etc/nginx/sites-available/yourcodeignitersite
```

### Add the following configuration:

```nginx
server {
    listen 80;
    server_name yourcodeignitersite.com www.yourcodeignitersite.com;

    root /var/www/yourcodeignitersitefolder/public;
    index index.php index.html;

    location / {
        try_files $uri $uri/ /index.php?$query_string;
    }

    location ~ \.php$ {
        include snippets/fastcgi-php.conf;
        fastcgi_pass unix:/run/php/php8.3-fpm.sock;
    }

    location ~ /\.ht {
        deny all;
    }
}
```
> For **CodeIgniter 3**, set `root /var/www/yourcodeignitersitefolder;` (no `/public`).

### Enable the configuration and reload Nginx:

```shell
ln -s /etc/nginx/sites-available/yourcodeignitersite /etc/nginx/sites-enabled/
nginx -t
systemctl reload nginx
```

---

## Secure CodeIgniter with SSL

Use Let’s Encrypt to enable HTTPS for free:

```shell
certbot --nginx -d yourcodeignitersite.com
```

---

If all goes fine, your CodeIgniter application should now be live at:  
👉 https://yourcodeignitersite.com

---

**Tip:**  
- For CodeIgniter 4, make sure your `public` folder is the web root.
- For CodeIgniter 3, the web root is the project root.

# About me
I’m a passionate web developer with experience in building scalable and secure applications. I specialize in Laravel, Django, JavaScript and server management with Nginx. I enjoy automating workflows, optimizing performance, and deploying modern web solutions on VPS environments. I believe in clean code, continuous learning, and sharing knowledge through open-source contributions.

## Connect & Support

[![LinkedIn](https://img.shields.io/badge/LinkedIn-Connect-blue?style=for-the-badge&logo=linkedin)](https://www.linkedin.com/in/firoz-ebna-jobaier)
[![Buy Me a Coffee](https://img.shields.io/badge/Buy_Me_a_Coffee-Support-yellow?style=for-the-badge&logo=buymeacoffee)](buymeacoffee.com/yenHunter)
[![Fork me on GitHub](https://img.shields.io/badge/Fork_on_GitHub-000?style=for-the-badge&logo=github)](https://github.com/yenHunter)