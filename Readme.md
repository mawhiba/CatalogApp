1. Deploying Python Web App to Linux server:
   In this project we will take baseline installation of a Linux server prepare it to host our web application (Item Catalog). We will secure our server from a number of attack vectors, install and configure a database server, and finally deploy Item Catalog web applications onto it.

2. Notes to Reviewer:
   - IP address:
       Public IP : 18.185.86.252
       Private IP : 172.26.4.233
       Login to Linux server using grader user : ssh -i grader grader@18.185.86.252 -p 2200
   - SSH port: 2200
   - URL : http://18.185.86.252.xip.io
   - grader user password : grader.
   - Software Installed:
     - Apache2
     - mod_wsgi
     - sqlalchemy
     - flask
     - psycopg2-binary
     - Pillow
     - oauth2client
     - flask_marshmallow
     - git
     - pip
     - postgresql
     - Python Virtual Environments
   - Third-parties:
     - Google API
     -  Amazon Lightsail AWS

3. Create server instance:
    Create an account in https://lightsail.aws.amazon.com.

4. Create grader user:
    - From your terminal enter to your virtual machine, and create a new user grader: sudo adduser grader.
    - Password : grader .
    - Give grader user the root privillegs : sudo cp /etc/sudoers.d/90-cloud-init-users /etc/sudoers.d/grader.
    - Enter the file : sudo nano /etc/sudoers.d/grader
    - Edit the vagrant user to grader, so it will be like this: grader ALL=(ALL:ALL) ALL

5. Create key pairs:
    - From your local inside the terminal type : ssh-keygen.
    - Copy the content of the public key: cat .ssh/grader-key.pub.
    - Back to grader user, then cd /home/ grader.
    - sudo mkdir .ssh
    - sudo touch .ssh/authorized_keys
    - sudo nano .ssh/authorized_keys
    - Paste the content of the public key inside.
    - Change the permission for the .ssh directory an the authorized_keys:
      sudo chmod 700 .ssh
      sudo chmod 644 .ssh/authorized_keys

6. Prevent Root Login using SSH:
    - sudo nano /etc/ssh/sshd_config
    - Change PermitRootLogin yes >> PermitRootLogin no
    - Add this line: AllowUsers grader.
    - Restart the ssh service: sudo service ssh restart.

7. Change the port for ssh to 2200:
   - Edit the file sshd_config and add this line Port 2200 under Port 20 line : sudo nano /etc/ssh/sshd_config.

8. Configure the Uncomplicated Firewall (UFW):
   - sudo ufw allow 2200/tcp
   - sudo ufw allow 80/tcp
   - sudo ufw allow 123/udp
   - sudo ufw enable

9. Configure the local timezone to UTC:  
   Set to UTC.
   - sudo dpkg-reconfigure tzdata.

10. Update all currently installed packages:
   - sudo apt-get update.
   - sudo apt-get upgrade

11. Install and configure Apache to serve a Python mod_wsgi application:
   - Install Apache2: sudo apt-get install apache2
   - Install mod_wsgi: sudo apt-get install python-setuptools libapache2-mod-wsgi
   - Restart Apache sudo service apache2 restart

12. Install and configure PostgreSQL:
   - Install PostgreSQL sudo apt-get install postgresql.
   - Check if no remote connections are allowed sudo vim /etc/postgresql/9.3/main/pg_hba.conf.
   - Login to user postgres : sudo su - postgres.
   - Type : psql.
   - Create a new database called catalogdb and new user called catalog:
      postgres=# CREATE DATABASE catalogdb;
      postgres=# CREATE USER catalog;
      postgres=# ALTER ROLE catalog;
      postgres=# GRANT ALL PRIVILEGES ON DATABASE catalogdb TO catalog;
      Quit postgreSQL postgres=# \q
      Exit from user "postgres" : exit

13. Install git, clone and setup your Catalog App project.
   - Install Git using sudo apt-get install git.
   - Create a folder catalog under the /var/www directory:
      cd /var/www
      sudo mkdir catalog
      cd catalog
   - Create another directory under catalog:
      cd catalog
      sudo mkdir catalog
      cd catalog
   - Clone the Catalog from my repository clone https://github.com/mawhiba/CatalogApp

14. Virtualenironment:
   - Install pyhton: sudo apt-get python
   - Install virtual environment for Python: sudo apt-get install python-venv
   - Create a virtual enviroment: sudo python -m venv Virtenv
   - Activate the environment source: Virtenv/bin/activate
   - Install Apache WSGI module for python sudo python -m pip install libapache-mod-wsgi-py

15. Installing Dependencies:
   - sudo python -m pip install sqlalchemy
   - sudo python -m pip install flask
   - sudo python -m pip install psycopg2-binary
   - sudo python -m pip install Pillow
   - sudo -H python -m pip install oauth2client

- Create a wsgi file under the /var/www/catalog:
  cd /var/www/catalog
  sudo touch flaskapp.wsgi
  sudo nano flaskapp.wsgi
  past this code:
  #!/usr/bin/python
  import sys
  import logging
  logging.basicConfig(stream=sys.stderr)
  sys.path.insert(0,"/var/www/FlaskApp/")

  from FlaskApp import app as application
  application.secret_key = 'super_secret_key'

- Create apache config file which will interact with the above file when the app is requested as an http:
  sudo touch /etc/apache2/sites-available/FlaskApp.conf
  sudo nano /etc/apache2/sites-available/FlaskApp.conf

  <VirtualHost*:80> ServerName 18.185.86.252.xip.io ServerAdmin 18.185.86.252 WSGIDaemonProcess catalog python-path=/var/www/catalog:/var/www/catalog/venv/lib/python2.7/sit$ WSGIProcessGroup catalog  WSGIScriptAlias / /var/www/catalog/flaskapp.wsgi WSGIPassAuthorization On <Directory /var/www/catalog/catalog/> Order allow,deny Allow from all Alias /static /var/www/catalog/catalog/static/ <Directory /var/www/catalog/catalog/static/> Order allow,deny Allow from all ErrorLog ${APACHE_LOG_DIR}/error.log # e.g. LogLevel debug LogLevel debug CustomLog ${APACHE_LOG_DIR}/access.log combined


16. Changes in python files:
     In project.py :
         app = Flask(__name__)
         APP_PATH = '/var/www/catalog/catalog/CatalogApp'
         app.secret_key = 'super_secret_key'

      - Add the path for json file:
           CLIENT_ID = json.loads(
           open('/var/www/catalog/catalog/client_secrets.json', 'r').read())['web']['client_id']
           APPLICATION_NAME = "Item Catalog"

      - Edit the engin line and add these line to database_setup.py file and categories.py file also:
          POSTGRES = { 'user': 'catalog', 'pw': 'mawhiba123', 'db': 'catalog', 'host': 'localhost', }
          engine = create_engine('postgresql://%(user)s:%(pw)s@%(host)s/%(db)s' % POSTGRES)

      - UPLOAD_FOLDER = '/var/www/catalog/catalog/static/image'
        APP_PATH = '/var/www/catalog/catalog/'

      - Remove the debug statment: app.debug = True

      - Change the run command to : app.run()

17. Google console configuration:
    To setup a DNS for our web app:
      - From OAuth consent screen, add a new authorized domain : xip.io
      - From Credentials screen:
        - Change the Authorized JAVAScript Origin to : http://18.185.86.252.xip.io
        - Change Authorized redirect URIs to :
          	http://18.185.86.252.xip.io/login
            http://10.185.86.252.xip.io/gconnect

18. Run the App:
    - Restart Apache : sudo apache2ctl restart sudo service apache2 restart
    - cd /var/www/catalog/catalog/CatalogApp
    - Created the database tables: python database_setup.py
    - Fill the database tables: python categories.py
    - ran the application file python: project.py
    - In the browser type this url: http://18.185.86.252.xip.io
