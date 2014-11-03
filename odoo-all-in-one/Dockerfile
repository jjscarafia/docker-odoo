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
RUN apt-get update && \
    apt-get install -y python-pip python-dev build-essential libpq-dev \
    poppler-utils antiword libldap2-dev libsasl2-dev libssl-dev git \
    python-dateutil python-feedparser python-gdata python-ldap python-lxml \
    python-mako python-openid python-psycopg2 python-pychart python-pydot \
    python-pyparsing python-reportlab python-tz python-vatnumber python-vobject \
    python-webdav python-xlwt python-yaml python-zsi python-docutils python-unittest2 \
    python-mock python-jinja2 libevent-dev libxslt1-dev libfreetype6-dev libjpeg8-dev \
    python-werkzeug wkhtmltopdf libjpeg-dev 

WORKDIR /opt/odoo/server/
RUN git checkout 8.0
RUN python setup.py install

RUN sudo -H -u odoo /opt/odoo/server/odoo.py --stop-after-init -s -c /opt/odoo/odoo.conf --db_host=localhost --db_user=odoo --db_password=odoo

USER postgres
RUN service postgresql start && psql -c "CREATE USER odoo WITH PASSWORD 'odoo';" && psql -c "ALTER USER odoo WITH SUPERUSER;"

USER root

CMD "service postgresql start && sudo -H -u odoo /opt/odoo/server/odoo.py -c /opt/odoo/odoo.conf"
