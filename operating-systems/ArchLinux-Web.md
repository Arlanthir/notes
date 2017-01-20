# Arch Linux - Web Development

## NGINX

```bash
sudo pacman -S nginx-mainline 
```

### Configure nginx to run as another user:
```
sudo nano /etc/nginx/nginx.conf
# Uncomment "user xxx;" to run as another user
```

## PHP + phpMyAdmin
```bash
sudo pacman -S php php-fpm php-mcrypt phpmyadmin

sudo nano /etc/php/php-fpm.conf

error_log = /var/log/php-fpm.log

# Change php-fpm to run as another user:

Add:

sudo nano /etc/php/php-fpm.d/www.conf
[www]
user = xxx
group = xxx

listen.owner = xxx
listen.group = xxx
;listen.mode = 0660

# Ensure the following modules are enabled:

bz2
curl
mcrypt
mysqli
pdo_mysql
zip
```

```bash
ln -s /usr/share/webapps/phpMyAdmin/ /home/<xxx>/dev/
```

## MySQL
```bash
sudo pacman -S mariadb
sudo mysql_install_db --user=mysql --basedir=/usr --datadir=/var/lib/mysql
sudo systemctl enable mariadb.service
sudo systemctl start mariadb.service
mysql_secure_installation
```
