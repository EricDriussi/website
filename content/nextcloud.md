## Before we go

Skip this if DB already set up.
We'll use mariadb.
Use link BUT this path might be borked, depends on system:
CHANGE /etc/php7/conf.d/mysql.ini FOR /etc/php/7.3/fpm/conf.d/10-mysqlnd.ini

Throw a `mysql_secure_installation` and follow along.

In mariadb:

```mysql
CREATE USER 'nextcloud-admin'@'localhost' IDENTIFIED BY 'admin-nextcloud';
CREATE DATABASE IF NOT EXISTS nextcloud CHARACTER SET utf8mb4 COLLATE utf8mb4_general_ci;
GRANT ALL PRIVILEGES on nextcloud.\* to 'nextcloud-admin'@'localhost';
FLUSH privileges;
```

## Now we go

Go [here](https://nextcloud.com/install/#instructions-server) and select the web installer.
Follow the instructions:

## Upload Setup to Server

ssh into server and

```bash
mkdir /var/www/nextcloud
rsync -vrp ~/downloads/setup-nextcloud.php root@unixmagick.xyz:/var/www/nextcloud
```

## Point Browser to Setup

### Serve Setup

If apache no problem
nginx doesn't support php natively
needs PHP-FPM
install php7.\*-fpm

NGINX:
-no index on server
-both locations / and ~ / need `fastcgi_pass unix:/run/php/php7.\*-fpm.sock;`

GENERAL SETUP: https://stackoverflow.com/a/26668444/14385510
502 - PHP CONFIG: https://www.datadoghq.com/blog/nginx-502-bad-gateway-errors-php-fpm/#nginx-cant-access-the-socket
FINALLY WORKED: https://stackoverflow.com/a/16887296/14385510

useful: https://docs.nextcloud.com/server/21/admin_manual/installation/nginx.html

## Follow Setup Instructions

Dependency check error, no permissions. Screenshot.
Solution: `chown www-data /var/www/nextcloud`

screenshot
screenshot

`open() "/usr/share/nginx/html/favicon.ico" failed`

esto nos dice que estaba tirandod el default
nginx se lia con los server{} si son más de uno, parece que el primero debe de tener el locations

ahora da un 502, actuar como antes y corregir
fastcgi_pass unix:/run/php/mypool.sock;
en la configuración de nginx para el sitio

## Login and create User/s

Useful links

-   [install](https://nextcloud.com/install)
-   [nginx conf](https://docs.nextcloud.com/server/21/admin_manual/installation/nginx.html)
-   [admin manual](https://docs.nextcloud.com/server/21/admin_manual/)
-   [database conf](https://docs.nextcloud.com/server/21/admin_manual/configuration_database/linux_database_configuration.html)

Useful logs

-   nvim /var/log/php7.3-fpm.log
-   nvim /var/log/nginx/error.log
-   nvim /var/log/nginx/access.log
