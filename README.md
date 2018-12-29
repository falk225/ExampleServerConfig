# ExampleServerConfig

#### IP Address
54.236.86.84

#### URL
http://54.236.86.84.xip.io/

### Installed Software
#### Linux (apt-get)
- finger
- mod-wsgi
- libapache2-mos-wsgi-py3
- apache2
- python3-pip
- python3-venv
- postgresql
- postgresql-contrib

#### Python (pip)
- flask
- sqlalchemy
- google-api-python-client
- oauth2client
- google_auth_oauthlib
- cryptography --upgrade
- psycopg2
### Configurations
#### /etc/ssh/sshd_config
- Change SSH port from 22 to 2200

#### Uncomplicated Firewall (UFW)
- default deny incoming
- default allow outgoing
- allow 2200/tcp
- allow www
- allow ntp
- ufw enable

#### Create/Provision grader user
- adduser grader
- cp /etc/sudoers.d/90-cloud-init-users /etc/sudoers.d/grader
- edit newly created grader file and change the user to grader
- add .ssh/authorized_keys file/path
- create key using ssh-keygen and add to authorized_keys

#### /etc/apache2/site-enabled/000-default.conf
Add to <VirtualHost>
```
        WSGIDaemonProcess ItemCatalog threads=5 python-home=/var/www/catalog/venv home=/var/www/catalog/ItemCatalog
        WSGIScriptAlias / /var/www/wsgi-scripts/run.wsgi
        <Directory /var/www/wsgi-scripts>
            <IfVersion < 2.4>
                Order allow,deny
                Allow from all
            </IfVersion>
            <IfVersion >= 2.4>
                Require all granted
            </IfVersion>
        </Directory>
        <Directory /var/www/catalog>
            <IfVersion < 2.4>
                Order allow,deny
                Allow from all
            </IfVersion>
            <IfVersion >= 2.4>
                Require all granted
            </IfVersion>
        </Directory>
```

#### Create .wsgi file
- add wsgi-scripts folder
- add run.wsgi
- edit and insert code
``` python
import sys
sys.path.insert(0, "/var/www/catalog/ItemCatalog")
from app import app as application
```

#### Lightsail Networking -> Firewall
Allow
- HTTP TCP 80
- Custom TCP 123
- Custom TCP 2200

### Resources
http://suite.opengeo.org/docs/latest/dataadmin/pgGettingStarted/firstconnect.html
https://www.compose.com/articles/using-postgresql-through-sqlalchemy/
https://www.digitalocean.com/community/tutorials/how-to-deploy-a-flask-application-on-an-ubuntu-vps#step-four-%E2%80%93-configure-and-enable-a-new-virtual-host
https://realpython.com/flask-by-example-part-2-postgres-sqlalchemy-and-alembic/
https://www.digitalocean.com/community/tutorials/how-to-install-and-use-postgresql-on-ubuntu-16-04
https://docs.sqlalchemy.org/en/latest/core/engines.html
