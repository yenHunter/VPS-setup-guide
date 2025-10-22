# Host Laravel Site

Laravel is a modern PHP framework designed for building robust, scalable web applications. Follow the steps below to deploy a Laravel project on your VPS using Nginx.

## Configure Laravel application
Follow the steps to configure your laravel appication on the VPS

### Create Laravel project directory

```shell
mkdir -p /var/www/yourlaravelsitefolder
cd /var/www/yourlaravelsitefolder
```

### Clone Laravel project from GitHub

```shell
git clone git@github.com:user_name/repo_name.git .
```

### Install dependencies

```shell
composer install
```

### Configure environment variables
Copy the example .env file and edit your configuration:

```shell
cp .env.example .env
nano .env
```

### Create MySQL database
Log into MySQL and create a new database with privileges.

```shell
mysql -u root -p
CREATE DATABASE yourlaravelsitedb;
GRANT ALL PRIVILEGES ON yourlaravelsitedb.* TO 'user_name'@'localhost';
FLUSH PRIVILEGES;
exit
```

### Run Laravel setup commands

```shell
php artisan migrate --seed
php artisan key:generate
php artisan config:cache
php artisan config:route
php artisan config:view
```

### Set correct permissions

```shell
chown -R www-data:www-data /var/www/yourlaravelsitefolder/*
chmod -R 775 /var/www/yourlaravelsitefolder/*
```

## Configure Nginx for Laravel

### Create a new site configuration file:

```shell
nano /etc/nginx/sites-available/yourlaravelsite
```

### Add the following configuration:

```shell
server {
    listen 80;
    server_name yourlaravelsite.com www.yourlaravelsite.com;

    root /var/www/yourlaravelsitefolder/public;
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

### Enable the configuration and reload Nginx:

```shell
ln -s /etc/nginx/sites-available/yourlaravelsite /etc/nginx/sites-enabled/
nginx -t
systemctl reload nginx
```

If all goes fine, your Laravel application should now be live at:
👉 http://yourlaravelsite.com

# About me
I’m a passionate web developer with experience in building scalable and secure applications. I specialize in Laravel, Django, JavaScript and server management with Nginx. I enjoy automating workflows, optimizing performance, and deploying modern web solutions on VPS environments. I believe in clean code, continuous learning, and sharing knowledge through open-source contributions.

## Connect & Support

[![LinkedIn](https://img.shields.io/badge/LinkedIn-Connect-blue?style=for-the-badge&logo=linkedin)](https://www.linkedin.com/in/firoz-ebna-jobaier)
[![Buy Me a Coffee](https://img.shields.io/badge/Buy_Me_a_Coffee-Support-yellow?style=for-the-badge&logo=buymeacoffee)](buymeacoffee.com/yenHunter)
[![Fork me on GitHub](https://img.shields.io/badge/Fork_on_GitHub-000?style=for-the-badge&logo=github)](https://github.com/yenHunter)
