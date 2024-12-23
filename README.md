ODOO 18 INSTALLATION GUIDE FOR UBUNTU
====================================

This guide will help you install Odoo 18 on Ubuntu. Follow these steps carefully.

1. SYSTEM PREREQUISITES
----------------------
Update system packages:
$ sudo apt update
$ sudo apt upgrade -y

Install required dependencies:
$ sudo apt install -y git postgresql postgresql-client python3.12 python3.12-dev python3.12-venv python3-pip node-less npm libldap2-dev libsasl2-dev libxml2-dev libxslt1-dev libjpeg-dev libpq-dev build-essential wget

2. CREATE POSTGRESQL USER
------------------------
$ sudo -u postgres createuser -s $USER

3. DOWNLOAD AND SETUP ODOO
-------------------------
Create directory:
$ mkdir ~/odoo18
$ cd ~/odoo18

Clone repository:
$ git clone https://github.com/odoo/odoo.git -b 18.0 --depth=1 .

Create virtual environment:
$ python3.12 -m venv .venv

Activate virtual environment:
$ source .venv/bin/activate

Install dependencies:
$ pip install --upgrade pip
$ pip install -r requirements.txt

4. CONFIGURE ODOO
----------------
Create configuration file:
$ mkdir ~/.odoo
$ touch ~/.odoo/odoo.conf

Add this configuration to odoo.conf:
[options]
addons_path = ~/odoo18/addons
db_host = False
db_port = False
db_user = $USER
db_password = False
http_port = 8069

5. CREATE LAUNCH SCRIPT
----------------------
Create run.sh:
$ cat > ~/odoo18/run.sh << EOF
#!/bin/bash
cd ~/odoo18
source .venv/bin/activate
./odoo-bin -c ~/.odoo/odoo.conf
EOF

Make it executable:
$ chmod +x ~/odoo18/run.sh

6. RUNNING ODOO
--------------
Start the server:
$ ~/odoo18/run.sh

7. ACCESS ODOO
-------------
- Open web browser
- Go to: http://localhost:8069
- Create your first database when prompted

COMMON ISSUES AND SOLUTIONS
--------------------------
1. Python.h missing:
   $ sudo apt-get install python3.12-dev

2. PostgreSQL access issues:
   $ sudo -u postgres createuser -s $USER

3. Permission issues:
   $ sudo chown -R $USER:$USER ~/odoo18

4. Port already in use:
   Edit ~/.odoo/odoo.conf and change http_port

DEVELOPMENT TIPS
---------------
1. Enable developer mode:
   Add /web?debug=1 to URL

2. Custom addons:
   $ mkdir ~/odoo18/custom-addons
   Add to odoo.conf:
   addons_path = ~/odoo18/addons,~/odoo18/custom-addons

3. Log configuration:
   Add to odoo.conf:
   logfile = ~/.odoo/odoo.log
   log_level = debug

USEFUL COMMANDS
--------------
Start with config:
$ ./odoo-bin -c ~/.odoo/odoo.conf

Update module:
$ ./odoo-bin -c ~/.odoo/odoo.conf -u module_name

Install module:
$ ./odoo-bin -c ~/.odoo/odoo.conf -i module_name

Developer mode:
$ ./odoo-bin -c ~/.odoo/odoo.conf --dev=all

IMPORTANT REMINDER
----------------
Always activate virtual environment before running Odoo:
$ source .venv/bin/activate

For more information, visit:
https://www.odoo.com/documentation/18.0/
