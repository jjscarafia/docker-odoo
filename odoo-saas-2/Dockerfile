FROM adhoc/odoo-base
MAINTAINER Damian Soriano <ds@ingadhoc.com>

ENV REFRESHED_AT 2014-09-17

USER root
RUN apt-get update && \
    apt-get install -y python-pip python-dev build-essential libpq-dev \
    poppler-utils antiword libldap2-dev libsasl2-dev libssl-dev git \
    python-dateutil python-feedparser python-gdata python-ldap python-lxml \
    python-mako python-openid python-psycopg2 python-pychart python-pydot \
    python-pyparsing python-reportlab python-tz python-vatnumber python-vobject \
    python-webdav python-xlwt python-yaml python-zsi python-docutils python-unittest2 \
    python-mock python-jinja2 libevent-dev libxslt1-dev libfreetype6-dev libjpeg8-dev \
    python-werkzeug wkhtmltopdf libjpeg-dev build-essential python-svn

# Links for PIL
RUN ln -s /usr/lib/x86_64-linux-gnu/libfreetype.so /usr/lib/
RUN ln -s /usr/lib/x86_64-linux-gnu/libz.so /usr/lib/
RUN ln -s /usr/lib/x86_64-linux-gnu/libjpeg.so /usr/lib/
RUN ln -s /usr/include/freetype2 /usr/include/freetype

WORKDIR /opt/odoo/server/
RUN git checkout saas-2
RUN python setup.py install

RUN sudo -H -u odoo /opt/odoo/server/openerp-server --stop-after-init -s -c /opt/odoo/odoo.conf --db_host=odoo-db --db_user=odoo --db_password=odoo

CMD ["sudo", "-H", "-u", "odoo", "/opt/odoo/server/openerp-server", "-c", "/opt/odoo/odoo.conf"]
