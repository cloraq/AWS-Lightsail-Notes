## Steps to redirect to a non www

1: Add a new record inside the Lightsail DNS Zone. type A, value =  WWW, Static IP (actual site)
2: Add the following rules in the /opt/bitnami/apps/wordpress/config/httpd-app-config

RewriteEngine On
RewriteBase /

# Redirect non-www to www

RewriteCond %{HTTP_HOST} ^www\.(.+)$ [NC]

RewriteRule ^ http%{ENV:protossl}://%1%{REQUEST_URI} [L,R=301] 

RewriteRule ^index\.php$ - [S=1]

RewriteCond %{REQUEST_FILENAME} !-f

RewriteCond %{REQUEST_FILENAME} !-d

RewriteRule . index.php [L]

3: Restar the apache with sudo /opt/bitnami/ctlscript.sh restart apache
