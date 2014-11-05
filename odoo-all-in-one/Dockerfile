FROM ubuntu:14.04
MAINTAINER Damian Soriano <ds@ingadhoc.com>

ENV REFRESHED_AT 2014-01-11

RUN apt-get clean
RUN apt-get update
RUN apt-get upgrade -y

RUN apt-get update && apt-get install -y postgresql-9.3 

RUN apt-get update && apt-get install -y git
RUN apt-get update && apt-get install -y python-pip

RUN mkdir -p /opt/odoo

RUN useradd -m -p odoo odoo
RUN chown -R odoo.odoo /opt/odoo/
USER odoo
RUN git clone https://github.com/odoo/odoo.git /opt/odoo/server

USER root
RUN apt-get install -y python-pip python-dev build-essential libpq-dev \
    poppler-utils antiword libldap2-dev libsasl2-dev libssl-dev git \
    python-dateutil python-feedparser python-gdata python-ldap python-lxml \
    python-mako python-openid python-psycopg2 python-pychart python-pydot \
    python-pyparsing python-reportlab python-tz python-vatnumber python-vobject \
    python-webdav python-xlwt python-yaml python-zsi python-docutils python-unittest2 \
    python-mock python-jinja2 libevent-dev libxslt1-dev libfreetype6-dev libjpeg8-dev \
    python-werkzeug wkhtmltopdf libjpeg-dev

#################################################3
## Git repositories and python dependencies
## TODO:remove the one you are not going to use
RUN mkdir -p /opt/odoo/sources
WORKDIR /opt/odoo/sources

RUN apt-get install -y swig
RUN pip install M2Crypto suds
RUN git clone https://github.com/ingadhoc/odoo-addons.git

RUN pip install geopy==0.95.1 BeautifulSoup
RUN git clone https://github.com/ingadhoc/odoo-argentina.git

RUN git clone https://github.com/ingadhoc/odoo-web.git

RUN apt-get install -y libcups2-dev
RUN pip install git+https://github.com/aeroo/aeroolib.git@master pycups

RUN git clone https://github.com/aeroo/aeroo_reports.git
RUN git clone https://github.com/OCA/server-tools
RUN git clone https://github.com/OCA/web

#################################################
## aeroo docs
RUN apt-get install -y python3-pip
RUN pip3 install daemonize jsonrpc2
RUN git clone https://github.com/aeroo/aeroo_docs.git /opt/odoo/aeroo_docs
#TODO correr el proceso de aeroo docs

WORKDIR /opt/odoo/server/
RUN git checkout 8.0
RUN python setup.py install

RUN sudo -H -u odoo /opt/odoo/server/odoo.py --stop-after-init -s -c /opt/odoo/odoo.conf --db_host=localhost --db_user=odoo --db_password=odoo --addons-path=/opt/odoo/server/openerp/addons,/opt/odoo/server/addons,/opt/odoo/sources/odoo-addons,/opt/odoo/sources/odoo-argentina,/opt/odoo/sources/odoo-web,/opt/odoo/sources/server-tools,/opt/odoo/sources/web,/opt/odoo/sources/aeroo_reports
#TODO correr un nginx para poder utilizar workers y el chat

################################################
USER postgres
RUN service postgresql start && psql -c "CREATE USER odoo WITH PASSWORD 'odoo';" && psql -c "ALTER USER odoo WITH SUPERUSER;"

#TODO ver si esto lo podemos evitar, son arreglos por encode utf8
RUN service postgresql start && psql -c "update pg_database set datallowconn = TRUE where datname = 'template0';"
RUN service postgresql start && psql -c "\c template0;"
RUN service postgresql start && psql -c "update pg_database set datistemplate = FALSE where datname = 'template1';"
RUN service postgresql start && psql -c "drop database template1;"
RUN service postgresql start && psql -c "create database template1 with template = template0 encoding = 'UTF8';"
RUN service postgresql start && psql -c "update pg_database set datistemplate = TRUE where datname = 'template1';"
RUN service postgresql start && psql -c "\c template1"
RUN service postgresql start && psql -c "update pg_database set datallowconn = FALSE where datname = 'template0';"

#TODO ver si podemos crear esta base de datos demo, la crea pero luego me da error, al replicarlo logueado me funciona bien
#RUN service postgresql start && createdb -O odoo demo_ar

################################################
USER root
#RUN service postgresql start && sudo -H -u odoo /opt/odoo/server/odoo.py --stop-after-init -c /opt/odoo/odoo.conf -i l10n_ar_base,adhoc_base_setup -d demo_ar --load-language=es_AR

CMD "service postgresql start && sudo -H -u odoo /opt/odoo/server/odoo.py -c /opt/odoo/odoo.conf"
