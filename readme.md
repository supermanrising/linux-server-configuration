#P5: Linux Server Configuration

Server IP address: 52.37.130.118

SSH Port: 2200

## Quick Start

The web application can be accessed by visiting [http://ec2-52-37-130-118.us-west-2.compute.amazonaws.com/](http://ec2-52-37-130-118.us-west-2.compute.amazonaws.com/)

## Summary of Configuration Changes

New user "grader" created

`adduser grader`

Grader given sudo access

Disabled root user login

Enforced key-based SSH authentication (see authentication section for SSH private keys)

Changed SSH port from 22 to 2200

`sudo nano /etc/ssh/sshd_config`

Configured UFW to only allow incoming connections for SSH (port 2200), HTTP (port 80), and NTP (port 123)

```
sudo ufw default deny incoming
sudo ufw default allow outgoing
sudo ufw allow 2200/tcp
sudo ufw allow 80/tcp
sudo ufw allow 123/tcp
sudo ufw enable
```

Updated all currently installed packages

```
sudo apt-get update
sudo apt-get upgrade
```

Installed and Enabled mod_wsgi

```
sudo apt-get install libapache2-mod-wsgi python-dev
sudo a2enmod wsgi
```

Installed postgresql

`sudo apt-get intsall postgresql postgresql-contrib`

Created new database for flask application

`sudo su - postgres`
`createdb sharealens`

Created new database user

**Note:** The instructions say to name this user "Catalog".  I named the database user "ShareAlens" before I read that.  Hopefully the different username is okay, otherwise I can resubmit after changing :)

`sudo adduser sharealens`

Updated sharealens password

`ALTER USER sharealens WITH PASSWORD 'salpass';`

Gave sharealens all access to sharealens database

`sudo --user=postgres psql -c "GRANT ALL ON DATABASE sharealens TO sharealens”`

Installed SQLAlchemy

`sudo apt-get install python-sqlalchemy`

Intsalled psycopg2

`sudo apt-get install python-psycopg2`

Updated /etc/hostname and /etc/hosts to contain matching host entries

Created directory to store Flask application

```
cd /var/www
sudo mkdir ShareAlens
cd ShareAlens
sudo mkdir FlaskApp
```

Installed git and cloned sharealens repository:

```
sudo apt-get install git
sudo git clone https://github.com/supermanrising/sharealens-postgres.git
```

Installed pip

`sudo apt-get install python-pip`

Used pip to install virtualenv

`sudo pip install virtualenv`

Created a virtual environment and installed Flask there

```
sudo virtualenv venv
source vent/bin/activate
sudo pip install Flask
deactivate
```

Created FlaskApp.conf within apache2 sites-available folder

`sudo nano /etc/apache2/sites-available/FlaskApp.conf`

Enabled virtual host

`sudo a2ensite FlaskApp`

Created .wsgi file to serve Flask app

```
cd ~/var/www/FlaskApp
sudo nano flaskapp.wsgi
```

Restarted apache

`sudo service apache2 restart`

## Authentication

### udacity_key.rsa

```
-----BEGIN RSA PRIVATE KEY-----
MIIEpQIBAAKCAQEA7iN+4kvGxZuDhi4lMZfCIpfNZDOe/3dshyByNItzYqZ4opdj
yHdg4oGTg64J/Z6AP7g9I4Keg5qdwfPjc1wdPL1Qg0580Tc/cvwDTM2vBpJx/AUn
gV4sycs5/uadq/OKRkWQtgNXw7MLe2+jv5UGWFNBk5sENqU+Gd4WEpMSMR+avCnf
/QzyeuHrv/tiyX5CQqj+CKK3PS7FnDrL2DHBGqYJX+9Xo4WP43R9C/4b9WyXO+Hj
4IVpjREYDrd7h4+vfBJ6wVoyKLQYtMMTUVNvdhuT21mrNBo9SSjOFlaS8XLHQNWG
xsEcHn9duXr94BN3g9Lis7mJhcjW27p+V3FreQIDAQABAoIBAQCpfs4y12h1AclN
Wc7TS4a8BHwGE8/ZWPEABJIE4DSSRJacc1BsQLvOvBd4pAksYQI7WDD7815LoWMj
xyle1HNi5gRGUTj52G7qsoDOy58F+Hn0vN5vb85FGsb+rLoQx1jlx2HGiv6OpgNI
Kh/Mno9Tkn6cRrtrAZX/51iig9dw6gQ8+8DkL+W8bMRUDFQLSb9NyWSujyf0Ou3Y
qMBVo8o/zYP7X2xKfCpivvuIbb/fuUhFCfc+eHqV1RgJfsXMLmfql2WBEoQG25oH
Bl5d352nHCk6ek3BPjIy/7cINd4KoduxmImY+GroxHSl2IR0y2MaHzWr6Uoq3f9W
/nsHntiZAoGBAPrCm6xR3yZOyrQwvZcHFmrIqGI/9xZ4zsRICkK23nTDdUwwfgHv
whA5bNGVurEb08211V464A2VaMxWGlfh6+hmhZYnzumHBnkPaA1KIc3FjsUtnIxf
ZtqT40QPVO0i6XiuGrqQAFAWcwHxZjjmM5QUO619B6nnycncqgiTMSvjAoGBAPMd
XwP0fwXDV1jsSKamkibs7mhC7Nt17wD9qQRWPcvX6R6xnSHLu76hUXkKRYXubzhT
15Iliw3kL+41/dVQo2SZdMnb5UiDpi1FAOaL+WF2oRFVi/1BwuuK/c8XMQfrzLi0
tgQ3tT1YGhI0sjNhHk+nYaULf3KwB4jIECcOGaHzAoGAKWdaluBJzSRzWb82fqpf
7C/Hhbl0DdTSpxwR/aP+JR9kzbiwBZfV3iHtMsnbMoUauruMSGvGNNf3ns2UufAG
qK/M3Ncj1fFCg1ik4JTd8gDtqub2E0NpUyvZ+ZHifuklzZRJu4YtVwvt32NBcqGn
4IpatDGRw18PNXJm7NWI5+sCgYEAloMylgJCudCsPTNb70Dk7xB0sTvt5BjpdVWV
1EeITrFHdGdF/uxhOa4qAKPpUvfBB8Bwj9yKcHk7a2El23DnF5siAO8Qzooi0ZgM
7K7wH/UP1ul9l7ek86rDY/jZtCu6PQg0P/w1SttGmMrjIIgZ+fqIq2Oo7dopb/dc
eLF0ER0CgYEA6AfeApRH3YZ8JgauwwfXlEsjo2e+QRzgk8oo8DwCyZgiZqReZrbG
O9iLSGLor4nfY4ww2X89dqH8qSD+8XVA2o8SEYKac6IIuft6UWF347ND+Tk78ba6
WYPkGjmb6K1suv/vDZbLKePoyFrmD7qDd0Dhr4LdkzEq7fISKq9btDQ=
-----END RSA PRIVATE KEY-----
```

### grader rsa key (password = 'password')

```
-----BEGIN RSA PRIVATE KEY-----
Proc-Type: 4,ENCRYPTED
DEK-Info: AES-128-CBC,50842D830909B48A9DDEDF82BBF772CF

9NUjsbKiYArrTjf2huh2QPEE5U5ifOvuj7JzVfqDoLRUISg7hUGb7di5LNhcWYvi
ix7Bc/PtnXkmDhgiiLAapDffWPs8wutSkBWk0MgbHsfbXQW5XZXndKViTXkcxoa1
Aak6fdeO7VmD8RzCiGgOy18hTlNjvJiQ2kov434AQiYT+EXxhLZJyjx+/Fldkukk
w95vGDyYNaaDs+tp7Mhn8R5b+KRQSplwn/Tl2PAGvmvqlzLyUyUSgJLdNzg9MQV6
haq0wR0EzPQbHAcuYYhlYoQ9brQQqHUaDYyLYt+lbM0TMcUa5pYIYnVD9HZv3HZi
IZYPedxilWUG/f8u2kRkk7r3aSIWkkffgp3fOOrw4adp50st3kM4aeW+NB+Q5aw2
k+GmNMTxZzvy85LoeswmRe/kXLxmAFOtpyxF8Jl0bPSDiUyC44wmz/c3lSF2fZws
zluN2t7QL1yWE3UbLklH5ryVt9VMU8GKDVnCUIua073np4/8NJTiDmXElOjTJ9va
f+op3qM3cqhzDT5XxHgbflhu15QNJdOO5a0tM0b4SHTkBWwVEbRs8iyHA/u1hElf
FfhDWVXVunl7JDm/xrgrjFj++uQQSYrkJP/L0720wwOpHKNJ/GXmKCc8j44ZtU5C
DKKMH/OvSCESU7/4KCVishERiiu0cl+OgViMF1K5tNwmaOoMLbqxth3ZTO9Xl2jd
DaLote6Aq2rgkZ5PohIAzSLla21yVc0EoSemFeP7kogEGTkk9GKBlqR6FEie1hwZ
jY4siBWZHCAI77mRLPCR02nfjtv/PT3PzZEmQ9Injmq2/fVolUmE6h9N3kNDsak/
sKTfBLw9xzHseChGpQ5PSwiNgE7L3z3hTk+HvdWDEqPOb/wMW04u+CvZwBZtesXX
r0sVnJ5avAmVP4HUaUtzLqlvEsR+zCPkm2QpIrdUlfgHmuIHiseTLfM7OBnG9JHs
2bcd1U8NQ84XUw5Y7eEu42oDJxO3yuLPD8szDCZ3vMgY9e3aiq/X0h1Rg//15oNn
o9DlFTcE9M/fEa7niuVERhGVLIFJ6f/r3a3SV2W7KLaFFqeYBw/Mz2WkgFyhVco8
5kYQnFOGI4fp6BWN3N+TqvnELnFAcatxo6MVR6beQ6vIJccSv4yMsFCEAIy4KGt6
QS2/vigkY6ozNBIclspgW69PSR5W9DbHIYLWk5sv1N6YiIeka8qCU5wX3OO54MpX
u9nBZDAjyQgb1ph7N++yWYDhBitiRZsB00FOaINSQUF0UpoaHJMMitUe0UH5fMSZ
Pd9FAPLUBCbgmfD283cvHEC6mHZsxZy8a3aBtKSHFtJfq3tLH1VbvIbjzojW9DJy
sraUVRcIBng5IOHIu+l9kJ9HI2iAuheW5R4vvxj2hSHat/B8V7lFKcdqqbnrXhq9
Tx0CaWoHeVZ3q57qt7Pw1sZiFAPIqIjV2oNzDKVSKsNALJ0p2zdBq8YOt+E47BKK
MvSnFl6YhQqs/xAipAPGOvzwZ6Wal7PE4w6H7fWak80fCc89kjtiB9/ixKCnuD6W
vnWJcDlZ2pXu1lg1nZPKGFh4UgmhwazaF/VyRMKlCopNvqOsP6ShkKyP7WtcDWHd
-----END RSA PRIVATE KEY——
```

## References

https://www.digitalocean.com/community/tutorials/how-to-deploy-a-flask-application-on-an-ubuntu-vps
https://www.digitalocean.com/community/tutorials/how-to-install-and-use-postgresql-on-ubuntu-14-04
https://www.digitalocean.com/community/tutorials/how-to-add-and-delete-users-on-an-ubuntu-14-04-vps
http://flask.pocoo.org/docs/0.10/deploying/mod_wsgi/
http://flask.pocoo.org/docs/0.10/config/
http://drumcoder.co.uk/blog/2010/nov/12/apache-environment-variables-and-mod_wsgi/
http://www.postgresql.org/docs/8.0/static/sql-alteruser.html
http://www.postgresql.org/docs/8.0/static/sql-grant.html
http://stackoverflow.com/questions/25026326/importerror-cannot-import-name-app
https://discussions.udacity.com/ (10+ discussions)