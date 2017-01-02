# Apache

## Add Virtual Hosts

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
