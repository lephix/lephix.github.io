---
# layout: post
---
Following is a simple reverse proxy configuration for Apache2. Put these codes in httpd.conf file.

~~~configuration
# This is setting is for overriding the default conf
<VirtualHost *:80>
    ServerName localhost
    <Directory "/Library/WebServer/Documents">
        Options FollowSymLinks Multiviews
        MultiviewsMatch Any
        AllowOverride None
        Require all granted
    </Directory>
</VirtualHost>

# A proxy for 8080 port
<VirtualHost *:80>
    ServerName localhost-toutiao
    ProxyPreserveHost On
    ProxyPass / http://localhost:8080/
    ProxyPassReverse / http://localhost:8080/
    #ErrorLog "/private/var/log/apache2/dummy-host2.example.com-error_log"
    #CustomLog "/private/var/log/apache2/dummy-host2.example.com-access_log" common
</VirtualHost>

# A proxy for 4000 port
<VirtualHost *:80>
    ServerName lephix
    ProxyPreserveHost On
    ProxyPass / http://localhost:4000/
    ProxyPassReverse / http://localhost:4000/
    #ErrorLog "/private/var/log/apache2/dummy-host2.example.com-error_log"
    #CustomLog "/private/var/log/apache2/dummy-host2.example.com-access_log" common
</VirtualHost>
~~~
