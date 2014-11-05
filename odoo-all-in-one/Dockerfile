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

RUN git clone https://github.com/ingadhoc/odoo-addons.git
RUN git clone https://github.com/ingadhoc/odoo-argentina.git
RUN pip install geopy==0.95.1 BeautifulSoup
RUN git clone https://github.com/ingadhoc/odoo-web.git
RUN git clone https://github.com/ingadhoc/aeroo_reports.git
RUN pip install genshi==0.6.1 http://launchpad.net/aeroolib/trunk/1.0.0/+download/aeroolib.tar.gz
RUN git clone https://github.com/OCA/server-tools
RUN git clone https://github.com/OCA/web

#################################################3
## aeroo docs
RUN apt-get install -y libcups2-dev python3-pip
RUN pip3 install daemonize jsonrpc2
RUN git clone https://github.com/aeroo/aeroo_docs.git /opt/odoo/aeroo_docs

WORKDIR /opt/odoo/server/
RUN git checkout 8.0
RUN python setup.py install

RUN sudo -H -u odoo /opt/odoo/server/odoo.py --stop-after-init -s -c /opt/odoo/odoo.conf --db_host=localhost --db_user=odoo --db_password=odoo --workers=9 --addons-path=/opt/odoo/server/openerp/addons,/opt/odoo/server/addons,/opt/odoo/sources/odoo-addons,/opt/odoo/sources/odoo-argentina,/opt/odoo/sources/odoo-web,/opt/odoo/sources/server-tools,/opt/odoo/sources/web,/opt/odoo/sources/aeroo_reports

USER postgres
RUN service postgresql start && psql -c "CREATE USER odoo WITH PASSWORD 'odoo';" && psql -c "ALTER USER odoo WITH SUPERUSER;"

USER root

CMD "service postgresql start && sudo -H -u odoo /opt/odoo/server/odoo.py -c /opt/odoo/odoo.conf"
