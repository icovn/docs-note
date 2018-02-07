\# check python version

python



\# install pip

curl https://bootstrap.pypa.io/get-pip.py -o get-pip.py

python get-pip.py



\# install virtualenv \(is a tool to create isolated Python environments\)

pip install virtualenv

pip install --upgrade pip



\# Django \(https://docs.djangoproject.com/en/1.10/faq/install/\#faq-python-version-support\)



\#\# Install Apache and mod\_wsgi \(for production\)

- Apache https://httpd.apache.org/

- http://modwsgi.readthedocs.io/en/develop/

- https://docs.djangoproject.com/en/1.10/howto/deployment/wsgi/modwsgi/



\#\# Get your database running



\#\# Install the Django code

virtualenv --python=/usr/bin/python3 /u01/projects/github/icovn/hello-django/virtualenv

source /u01/projects/github/icovn/hello-django/virtualenv/bin/activate

pip install Django



\#\# Test Django

python

&gt;&gt;&gt; import django

&gt;&gt;&gt; print\(django.get\_version\(\)\)



\#\# Get Django version

python -m django --version





\#\# Creating a project \(https://docs.djangoproject.com/en/1.10/intro/tutorial01/\)

django-admin startproject myDjango



\#\# Migrate

cd myDjango

python manage.py migrate



\#\# Run development server

python manage.py runserver

python manage.py runserver 8080

python manage.py runserver 0.0.0.0:8000





\#\# Setup MySQL

- run: 

	+ sudo apt-get install libmysqlclient-dev python-dev python3-dev 

	+ pip install mysqlclient

	+ python manage.py migrate

- change configuration in myDjango/settings.py



\#\# Creating a app

- run: python manage.py startapp polls

- add models

- add config

- run: 

	+ python manage.py makemigrations polls

	+ python manage.py sqlmigrate polls 0001

	+ python manage.py migrate



\#\# Creating an admin user

python manage.py createsuperuser



\#\# Test

python manage.py test polls



\#\# Get Django source folder

python -c "import django; print\(django.\_\_path\_\_\)"



\#\# Promgen



\#\#\# Setup enviroment

cd /u01/applications/promgen/promgen-0.25

virtualenv --python=/usr/bin/python3 /u01/applications/promgen/virtualenv

source /u01/applications/promgen/virtualenv/bin/activate

pip install -e .\[dev\]

pip install mysqlclient

pip install Celery

mkdir logs



\#\#\# Setup data

promgen bootstrap

promgen migrate

promgen test

promgen createsuperuser



\#\#\# Setup static files

promgen collectstatic



\#\#\# Run web

promgen runserver --insecure



\#\#\# Run worker

celery -A promgen worker --loglevel=debug



\#\#\# Run other apps

cd /u01/applications/prometheus-2.1.0/

./prometheus --web.enable-lifecycle



cd /u01/applications/alertmanager-0.13.0/

./alertmanager



cd /u01/applications/blackbox\_exporter-0.11.0/

./blackbox\_exporter

