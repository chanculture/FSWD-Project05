Christopher Chan: Full Stack Web Developer Nanodegree
==========

Project 5: Linux Server Configuration
==========
As of April 25, 2016 Item Catalog is now Project 7 (previously project 5).

AWS EC2 server
==========
i.
----------
IP Address: 52.11.221.138
Domain name: http://ec2-52-11-221-138.us-west-2.compute.amazonaws.com/
SSH Port: 2200

ii.
----------
Project URL: http://ec2-52-11-221-138.us-west-2.compute.amazonaws.com/

iii. Summary of Software installed & configuration changes:
----------
ubuntu packages
----------
* git
* python-flask
* python-flask-sqlalchemy
* python-httplib2
* python-oauth2client
* python-psycopg2
* python-pip
* virtualenv
* libapache2-mod-wsgi
* libapache2-mod-wsgi python-dev
* subversion
* apache2
* postgresql

Configuration Changes:
----------
 
/etc/ssh/sshd_config
----------
+Port 2200
-Port 22
+PermitRootLogin no
+DenyUsers root
+AllowUsers grader admin catalog
+PasswordAuthentication no
-PasswordAuthentication yes

/etc/apache2/sites-available/Catalog.conf
----------
* Add:
<VirtualHost *:80>
                ServerName ec2-52-11-221-138.us-west-2.compute.amazonaws.com
                ServerAdmin chanculture@gmail.com
                WSGIScriptAlias / /var/www/Catalog/catalog.wsgi
                <Directory /var/www/Catalog/catalog/>
                        Order allow,deny
                        Allow from all
                </Directory>
                Alias /static /var/www/Catalog/catalog/static
                <Directory /var/www/Catalog/catalog/static/>
                        Order allow,deny
                        Allow from all
                </Directory>
                ErrorLog ${APACHE_LOG_DIR}/error.log
                LogLevel warn
                CustomLog ${APACHE_LOG_DIR}/access.log combined
</VirtualHost>

/var/www/Catalog/catalog.wsgi
----------
* Add:
#!/usr/bin/python
import sys
import logging
logging.basicConfig(stream=sys.stderr)
sys.path.insert(0,"/var/www/Catalog/catalog")

from project import app as application
application.secret_key = 'SECRET' # intentionally hidden

/etc/hosts
----------
* Add (resolve error: "sudo: unable to resolve host ip-10-20-3-122"):
127.0.0.1 localhost ip-10-20-3-122

/etc/apache2/sites-enabled/000-default.conf
----------
* Add:
<Directory /var/www/html>
            Options Indexes FollowSymLinks MultiViews
            AllowOverride All
            Order allow,deny
            allow from all
</Directory>

/var/www/html/.htaccess
----------
RewriteEngine on 
RewriteCond %{HTTP_HOST} ^52\.11\.221\.138$ RewriteRule ^/(.*)$ http://ec2-52-11-221-138.us-west-2.compute.amazonaws.com//$1 [L,R]

Configure local time zone
----------
sudo dpkg-reconfigure tzdata

iv. 3rd Party resources to help complete project
----------
* https://discussions.udacity.com/t/markedly-underwhelming-and-potentially-wrong-resource-list-for-p5/8587
* http://flask.pocoo.org/docs/0.10/deploying/
* https://www.digitalocean.com/community/tutorials/how-to-deploy-a-flask-application-on-an-ubuntu-vps
* https://www.seanw.org/blog/download-git-repo-subdirectory/
* https://www.digitalocean.com/community/tutorials/how-to-set-up-mod_rewrite-for-apache-on-ubuntu-14-04
* https://discussions.udacity.com/t/public-ip-for-google-oauths-authorized-redirect-uris/20341/3
* https://discussions.udacity.com/t/invalidclientsecretserror-error-opening-file-g-client-secrets-json-no-such-file-or-directory-2/163046/4

License
==========
Copyright Christopher Chan

