1. Deploying Python Web App to Linux server:
   In this project we will take baseline installation of a Linux server prepare it to host our web application (Item Catalog). We will secure our server from a number of attack vectors, install and configure a database server, and finally deploy Item Catalog web applications onto it.

2. Notes to Reviewer:
   - IP address:
       Public IP : 18.185.86.252
       Private IP : 172.26.4.233
       Login to Linux server using grader user : ssh -i grader grader@18.185.86.252 -p 2200
   - Private IP:

        -----BEGIN RSA PRIVATE KEY-----
        MIIEpAIBAAKCAQEA28u0y+VZqLU5FuTDAz0VjrSjkrtGiennNoaSZp2hLUUq255s
        9XuODQhAEHWWvCe8uCREKUqrW0dvOkEyu8BU1wITxuttomTGXyRYBF5pqosjN4Sq
        lDn+CP467IoumFOqOXXSTo03Q1+yFYNPDbEx3BIbn6sMa1viUqUSe1tkNGQC0wpx
        SObwwHymArXbRC35yeSrBzrqmTlpYPkzgK8H/lCgLfCj0O320PH1uJS9I1Tb2/cL
        RuPHQu0JAeMI/S4wjjWT/8Bb8Qwv7/pvwc+TFGPlYCBSE07dv0XfFVsiXmAUTHbv
        LQLDEoXqpJZJ7wn7uX/gyWJD5vrBQM3DG23OZQIDAQABAoIBAA0FzcmC5kQLsL09
        gpxvgxZ4d3SbKfvD4xIk+Qmhb0pKXdazVUtaLblx8rHG9c5iIFlctLkplmuLGPj/
        oezj4WL32YogqtdDV0mN0rU3jtUu90Az2RN9OCL6RS18pnQaCOzsoTBMR6jzQ47o
        v8dU51pdrEtSjCLUR92TsDuk15QO+Muii8KKW8TxNkIAVRquoRgeAdydiwTk8R1V
        ZQ5V9tiA/6z7vMQUHBoAlMY41Fy4F9RPsRof9WOlP19dmnVXhoEHW4/R0LGqox2D
        iPcVvpeeGdItx4m1udNl/IhKMxXlgA6duaxcyl3YJX98nVIkVkUNFkkL/8x/laj7
        TM1RqYECgYEA72Pia7wnTuBTxj/vEj2RQkYW5i49QMjtbBteUDJdjZoVavxXzFqA
        +5m8rsVGfCz28lc0ZVSUI7WuGn6fxokEK2wcGoZyjk0pJRoItbLHVweiCsdJtT+Z
        PgB2AKL4dHsBfHVSNFmow+v1upnBEeqIMiOqj/iNL/bGKxoyzZAROMUCgYEA6wvH
        m7qgfmLlnSi6lOAgp4LO5B9gLzlSx4WnY30PZ+aU/Y0k9L84B2evuI7bXjLLDa/G
        +OWEWQNIGDR62U0zlv8I4zYZF7ZCUsDvpsRJ2sgUf8uQSSGvdRTcZl97T81PkIPC
        JdJVAQVNLozWFqyD3rkD+IdejH0RszAaBHglWSECgYEAuaPVxAIm4W3oyUZvGNAR
        wzkjLOESsxidtUYL/1jvX43rqgmB9IECoRn5TfbB0C32Wrvxb9sE/iQ3nWgwcv9x
        8lJmANPeJigEDjeAJZc8KmtK59zfdeSZ65Dj8V5wGbQB1QgN9XbJ/xUOe+Qb6s7b
        SzaJYBg5NMbDqk0otGxSTpkCgYEAhAke/UUuSTEnCUzK2zy4O0FJFW7mqkCKVtim
        ukWTdOtbwL8cBnywbcB6PrPJWTYxSKP4ovLTkOk+A5Lfe9hZZbYoePJ30BErWq9V
        MJpNXqBMm6lviRVqKdwpHzz92UtdKbJKStLHu4YN4FtvHFeW0HOgXmk03LiZS852
        nxIa8OECgYBATmNxpHXjnIS8NTKWeMz+YM0FbNNkFHT8NiAo8oGUNa/t/AsaHa7i
        /4eQsr5fiyKCfbEafblP7RQN3+fAWFSZevFCemK8O+xTliHuibUPMq+Hp0FNY3Qf
        U3QVzxZcGFK9dGJg8phqmIJx46CIr4z9Dqo2fe+DxGh+yBHkcvrDjA==
        -----END RSA PRIVATE KEY-----
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
   - Create a new database called catalog and new user called catalog:
      postgres=# CREATE DATABASE catalog;
      postgres=# CREATE USER catalog;
      postgres=# ALTER ROLE catalog;
      postgres=# GRANT ALL PRIVILEGES ON DATABASE catalog TO catalog;
      Quit postgreSQL postgres=# \q
      Exit from user "postgres" : exit

13. Install git, clone and setup your Catalog App project.
   - Install Git using sudo apt-get install git.
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

- Create a wsgi file under the /var/www/:
  cd /var/www
  sudo touch flaskapp.wsgi
  sudo nano flaskapp.wsgi
  past this code:
      import sys
      import logging

      logging.basicConfig(stream=sys.stderr)
      sys.path.insert(0,"/var/www/CatalogApp")

      from project import app as application

      application.secret_key = 'super_secret_key'

- Create apache config file which will interact with the above file when the app is requested as an http:
  sudo touch /etc/apache2/sites-available/000-default.conf
  sudo nano /etc/apache2/sites-available/000-default.conf

  <VirtualHost*:80>
	ServerName 18.185.86.252.xip.io
	ServerAdmin 18.185.86.252

	#WSGIDaemonProcess catalog python-path=/var/www/catalog:/var/www/catalog/venv/lib/python2.7/sit$
	#WSGIProcessGroup catalog

	WSGIScriptAlias / /var/www/flaskapp.wsgi

	#WSGIPassAuthorization On
	<Directory /var/www/CatalogApp>
		Order allow,deny
		Allow from all
	</Directory>
	Alias /static /var/www/CatalogApp/static/

	<Directory /var/www/CatalogApp/static/>
		Order allow,deny
		Allow from all
	</Directory>

	ErrorLog ${APACHE_LOG_DIR}/error.log
	# e.g. LogLevel debug
	LogLevel debug
	CustomLog ${APACHE_LOG_DIR}/access.log combined
 </VirtualHost>

16. Changes in python files:
     In project.py :
         app = Flask(__name__)
         APP_PATH = '/var/www/CatalogApp'
         app.secret_key = 'super_secret_key'

      - Add the path for json file:
           APP_PATH = '/var/www/CatalogApp/'
           CLIENT_ID = json.loads(
               open(APP_PATH + 'client_secrets.json', 'r').read())['web']['client_id']
           APPLICATION_NAME = "Catalog App 2"

      - Edit the engin line and add these line to database_setup.py file and categories.py file also:
          POSTGRES = { 'user': 'catalog', 'pw': 'mawhiba123', 'db': 'catalog', 'host': 'localhost', }
          engine = create_engine('postgresql://%(user)s:%(pw)s@%(host)s/%(db)s' % POSTGRES)

      - UPLOAD_FOLDER = '/var/www/static/image'
        APP_PATH = '/var/www/CatalogApp'

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
