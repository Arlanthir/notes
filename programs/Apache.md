# Apache

## Installation - MacOS

OS X already includes Apache2.4, enable it by running `sudo apachectl start`
(stop it and disable it by running `sudo apachectl stop`)

Logs should be in `/private/var/log/apache2/`

A good idea is to keep your vhosts in `/Library/WebServer/Documents/` by setting up symlinks like so:
```bash
sudo ln -s /Users/<user>/dev/<site> /Library/WebServer/Documents/<site>
```

### Enable PHP

Edit `/etc/apache2/httpd.conf`:
Uncomment `LoadModule php5_module libexec/apache2/libphp5.so`


### Install MySQL

Download from http://dev.mysql.com/downloads/mysql/

Start MySQL
`sudo /usr/local/mysql/support-files/mysql.server start`

Change root password
`/usr/local/mysql/bin/mysqladmin -u root password 'yourpasswordhere'`

Fix the 2002 Socket Error
```
sudo mkdir /var/mysql
sudo ln -s /tmp/mysql.sock /var/mysql/mysql.sock
```

Auto-start MySQL on boot
`sudo nano /Library/LaunchDaemons/com.mysql.mysql.plist`
```xml
<!--?xml version="1.0" encoding="UTF-8"?-->
<plist version="1.0">
     <dict>
          <key>KeepAlive</key>
          <true />
          <key>Label</key>
          <string>com.mysql.mysqld</string>
          <key>ProgramArguments</key>
          <array>
               <string>/usr/local/mysql/bin/mysqld_safe</string>
               <string>--user=mysql</string>
          </array>
     </dict>
</plist>
```

```bash
sudo chown root:wheel /Library/LaunchDaemons/com.mysql.mysql.plist
sudo chmod 644 /Library/LaunchDaemons/com.mysql.mysql.plist
sudo launchctl load -w /Library/LaunchDaemons/com.mysql.mysql.plist
```

### Install phpMyAdmin
Download from http://www.phpmyadmin.net/home_page/downloads.php  
Extract it to ~/dev/phpmyadmin

```bash
mkdir ~/dev/phpmyadmin/config
chmod o+w ~/dev/phpmyadmin/config
```

Complete the setup at http://localhost/phpmyadmin/setup/  
(Add server, Authentication)

Configure PHP
`sudo cp /etc/php.ini.default /etc/php.ini`


## Configuration

### Add Virtual Hosts

Add the new host to your hosts file:
```
C:\Windows\system32\drivers\etc\hosts (Windows)
/etc/hosts (Linux / OS X)
```

```xml
<VirtualHost *:80>
    ServerAdmin webmaster@mysite.loc
    DocumentRoot "/var/www/mysite"
    ServerName mysite.loc
    ErrorLog "logs/mysite-error.log"
    CustomLog "logs/mysite-access.log" common
    <Directory "/var/www/mysite">
        AllowOverride All
        Require all granted
    </Directory>
</VirtualHost>
```

## Disable MultiViews
MultiViews translates /a/b/c to /a/b.php if folder b doesn't exist.  
Search for Options in httpd.conf and make sure the line doesn't have MultiViews

## Enable gzip compression (put in .htaccess)

```
AddOutputFilterByType DEFLATE text/plain
AddOutputFilterByType DEFLATE text/html
AddOutputFilterByType DEFLATE text/xml
AddOutputFilterByType DEFLATE text/css
AddOutputFilterByType DEFLATE application/xml
AddOutputFilterByType DEFLATE application/xhtml+xml
AddOutputFilterByType DEFLATE application/rss+xml
AddOutputFilterByType DEFLATE application/javascript
AddOutputFilterByType DEFLATE application/x-javascript
```
