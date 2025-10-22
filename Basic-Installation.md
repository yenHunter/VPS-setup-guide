# VPS Basic Installation
Welcome to VPS basic installation setup guide.

## Update & Upgrade

This will update and upgrade existing librarys and applications of Ubuntu (LTS)

```shell
apt update && apt upgrade -y
```

### Install nginx

Nginx is a web server that can also be used as a reverse proxy, load balancer, mail proxy and HTTP cache. The software was created by Russian developer Igor Sysoev and publicly released in 2004. Nginx is free and open-source software, released under the terms of the 2-clause BSD license.

```shell
apt install nginx -y
```

### Install MySQL server

MySQL is an open-source relational database management system (RDBMS). Its name is a combination of "My", the name of co-founder Michael Widenius's daughter My and "SQL", the acronym for Structured Query Language. A relational database organizes data into one or more data tables in which data may be related to each other; these relations help structure the data.

```shell
apt install mysql-server -y
```

### Install PHP with extention

PHP is a general-purpose scripting language geared towards web development. It was originally created by Danish-Canadian programmer Rasmus Lerdorf in 1993 and released in 1995. The PHP reference implementation is now produced by the PHP Group. PHP was originally an abbreviation of Personal Home Page, but it now stands for the recursive backronym PHP: Hypertext Preprocessor.

```shell
apt install php php-fpm php-mysql php-cli php-mbstring php-xml php-curl php-zip php-bcmath php-intl php-gd php-json php-tokenizer php-common -y
```

### Install Unzip, Git, Curl, Ufw, Composer
- Unzip – Extracts compressed .zip files.
- Git – Version control system for tracking code changes.
- Curl – Transfers data from or to a server via command line.
- UFW – Simplifies firewall management in Linux.
- Composer – Dependency manager for PHP projects.

```shell
apt install unzip -y
```
```shell
apt install git -y
```
```shell
apt install curl -y
```
```shell
apt install ufw -y
```
```shell
apt install composer -y
```

## MySQL User Configuration

After installing MySQL, you should secure your database by setting a strong root password, running the security script, and creating a new database user.

```shell
mysql
```

# Set root password

This command changes the root user’s password using the secure caching_sha2_password authentication method.

```shell
ALTER USER 'root'@'localhost' IDENTIFIED WITH caching_sha2_password BY 'your_strong_password';
exit
```

# Run MySQL security script

This script helps improve database security by removing test databases, anonymous users, and disabling remote root login.

```shell
mysql_secure_installation
```

# Create new MySQL user

Log back into MySQL and create a new user with a secure password.

```shell
mysql -u root -p
CREATE USER 'user_name'@'localhost' IDENTIFIED WITH caching_sha2_password BY 'your_strong_password';
exit
```

## Install and Configure phpMyAdmin

phpMyAdmin is a free and open-source administration tool for managing MySQL and MariaDB through a web interface. It allows you to perform database operations such as creating, modifying, and deleting databases, tables, and users easily.

```shell
apt install phpmyadmin -y
```

After installation, configure Nginx to serve phpMyAdmin on a dedicated subdomain.

```shell
nano /etc/nginx/sites-available/subdomain_for_phpmyadmin.yourdomain.com
```

Paste the following configuration:

```shell
server {
    listen 80;
    server_name subdomain_for_phpmyadmin.yourdomain.com;

    root /usr/share/phpmyadmin;
    index index.php index.html index.htm;

    location / {
        try_files $uri $uri/ =404;
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

Enable the configuration and reload Nginx:

```shell
ln -s /etc/nginx/sites-available/subdomain_for_phpmyadmin.yourdomain.com /etc/nginx/sites-enabled/
nginx -t && systemctl reload nginx
```
