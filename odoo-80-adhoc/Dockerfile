FROM adhoc/odoo:8.0
MAINTAINER Damian Soriano <ds@ingadhoc.com>

ENV REFRESHED_AT 2014-09-17

USER root

## Install postgres
RUN apt-get install -y postgresql

## Install odoo
WORKDIR /opt/odoo/server/
RUN git checkout 8.0
RUN python setup.py install

## addons
RUN mkdir -p /opt/odoo/sources
WORKDIR /opt/odoo/sources

# Usual adhoc generic addons
RUN git clone https://github.com/ingadhoc/odoo-addons.git
RUN git clone https://github.com/ingadhoc/odoo-argentina.git
RUN pip install geopy==0.95.1 BeautifulSoup
RUN git clone https://github.com/ingadhoc/odoo-web.git
RUN git clone https://github.com/ingadhoc/aeroo_reports.git
RUN pip install genshi==0.6.1 http://launchpad.net/aeroolib/trunk/1.0.0/+download/aeroolib.tar.gz

# Usual OCA addons
RUN git clone https://github.com/OCA/server-tools
RUN git clone https://github.com/OCA/web

## Create conf file
RUN sudo -H -u odoo /opt/odoo/server/odoo.py --stop-after-init -s -c /opt/odoo/odoo.conf --db_host=localhost --db_user=odoo --db_password=False --workers=9 --addons-path=/opt/odoo/server/openerp/addons,/opt/odoo/server/addons,/opt/odoo/sources/odoo-addons,/opt/odoo/sources/odoo-argentina,/opt/odoo/sources/odoo-web,/opt/odoo/sources/server-tools,/opt/odoo/sources/web,/opt/odoo/sources/aeroo_reports

CMD ["sudo", "-H", "-u", "odoo", "/opt/odoo/server/odoo.py", "-c", "/opt/odoo/odoo.conf"]
