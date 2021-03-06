# Arch Linux - Web Development

## NGINX

```bash
sudo pacman -S nginx-mainline
sudo systemctl enable nginx.service
sudo systemctl start nginx.service
```

### Configure nginx
```
sudo nano /etc/nginx/nginx.conf

# Uncomment "user xxx;" to run as another user
error_log /var/log/nginx/error.log notice;

# edit server { location / { root
```

### Add PHP support
```bash
sudo nano /etc/nginx/nginx.conf

location ~ \.php$ {
  fastcgi_pass unix:/var/run/php-fpm/php-fpm.sock;
  fastcgi_index index.php;
  root /srv/http;
  include fastcgi.conf;
}
```

## PHP + phpMyAdmin
```bash
sudo pacman -S php php-fpm php-mcrypt phpmyadmin

sudo nano /etc/php/php-fpm.conf

error_log = /var/log/php-fpm.log

sudo systemctl enable php-fpm.service
sudo systemctl start php-fpm.service
```

### Configure php-fpm to run as another user
```bash
sudo nano /etc/php/php-fpm.d/www.conf
[www]
user = xxx
group = xxx

listen.owner = xxx
listen.group = xxx
;listen.mode = 0660
```
### Configure modules
```bash
nano /etc/php/php.ini

# Ensure the following PHP modules are enabled

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
## NodeJS

Installing global node_modules on /usr/lib may conflict with web packages installed from pacman/AUR.

Add to .bashrc:
```bash
PATH="$HOME/.node_modules/bin:$PATH"
export npm_config_prefix=~/.node_modules
```

