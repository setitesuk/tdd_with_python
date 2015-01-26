Provisioning a new site
=======================

## Required packages

* nginx
* Python 3
* Git
* pip
* virtualenv

eg, on Ubuntu:

	sudo apt-get install nginx git python3 python3-pip
	sudo pip3 install virtualenv

## Nginx Virtual Host config

* see nginx.template.conf
* replace SITENAME with, eg, staging.my-domain.com

## Upstart Job

* see gunicorn-upstart.template.conf
* replace SITENAME with, eg, staging.my-domain.com

## Folder structure:
Assume we have a user account at /home/username

/home/username
|-- sites
		|--SITENAME
				|-- database
				|-- source
				|-- static
				|-- virtualenv

## Manual Deploy
git clone repo source
virtualenv --python=python3 ../virtualenv
../virtualenv/bin/pip install -r requirements.txt
../virtualenv/bin/python3 manage.py migrate --noinput
../virtualenv/bin/python3 manage.py collectstatic --noinput
../virtualenv/bin/python3 manage.py runserver
Ctrl-C
sudo cp deploy_tools/nginx.template.conf /etc/nginx/sites-available/$SITENAME
sudo ln -s /etc/nginx/sites-available/$SITENAME /etc/nginx/sites-enabled/$SITENAME
sudo service nginx start
sudo cp deploy_tools/gunicorn-upstart.template.conf /etc/init/gunicorn-$SITENAME.conf
start gunicorn-$SITENAME