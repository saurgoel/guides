To restart Apache 2 web server over the ssh, enter:
# /etc/init.d/apache2 restart

To stop Apache 2, enter:
# /etc/init.d/apache2 stop

To start Apache 2, enter:
# /etc/init.d/apache2 start

If you are using RHEL / CentOS / Fedora based server, enter:
# service httpd restart

To stop RHEL / CentOS / Fedora based Apache server, enter:
# service httpd stop

To start RHEL / CentOS / Fedora based Apache server, enter:
# service httpd start



# =========================== redirects

<VirtualHost *:80>
	ServerName www.domain1.com
	Redirect / http://www.domain2.com
</VirtualHost>



# ========================== HTTPACCESS REDIRECT
httpaccess redirect is different from the virtual host redirect


.Create two VirtualHost for two domains and use 301 redirect:

NameVirtualHost *:80
<VirtualHost *:80>
    ServerName example.com
    DocumentRoot "/path/to/site"
</VirtualHost>
<VirtualHost *:80>
    ServerName www.example.com
    Redirect 301 / http://example.com/
</VirtualHost>
2.Create two VirtualHost for two domains and use .htaccess with redirect-rule:

<VirtualHost *:80>
    ServerName example.com
    DocumentRoot "/path/to/site1"
</VirtualHost>
<VirtualHost *:80>
    ServerName www.example.com
    DocumentRoot "/path/to/site2"
</VirtualHost>
and create /path/to/site2/.htaccess with

Redirect 301 / http://example.com/
3.Create 1 VirtualHost and set redirect in common .htaccess:

<VirtualHost *:80>
    ServerName example.com
    ServerAlias www.example.com
    DocumentRoot "/path/to/site"
</VirtualHost>
create /path/to/site/.htaccess with

RewriteEngine On
RewriteCond %{HTTP_HOST} ^www.example.com$ [NC]
RewriteRule ^(.*)$ http://example.com/$1 [R=301,L]