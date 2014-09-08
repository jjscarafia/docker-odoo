FROM adhoc/odoo:8.0
MAINTAINER Damian Soriano <ds@ingadhoc.com>

ENV REFRESHED_AT 2014-08-30

RUN mkdir -p /opt/odoo/sources
WORKDIR /opt/odoo/sources

# ADHOC addons
RUN git clone -b master https://github.com/ingadhoc/odoo-addons.git
RUN git clone -b master https://github.com/ingadhoc/odoo-argentina.git
RUN pip install geopy==0.95.1 BeautifulSoup
RUN git clone -b master https://github.com/ingadhoc/odoo-academic

# Checkout specific 
RUN git --work-tree=/opt/odoo/sources/odoo-academic --git-dir=/opt/odoo/sources/odoo-academic/.git checkout 1.0.0

RUN sudo -H -u odoo /opt/odoo/server/odoo.py --stop-after-init -s -c /opt/odoo/server/odoo.conf --db_host=odoo-db --db_user=odoo --db_password=odoo --addons-path=/opt/odoo/server/openerp/addons,/opt/odoo/server/addons,/opt/odoo/sources/odoo-addons,/opt/odoo/sources/odoo-argentina,/opt/odoo/sources/odoo-academic/addons

CMD ["sudo", "-H", "-u", "odoo", "/opt/odoo/server/odoo.py", "-c", "/opt/odoo/server/odoo.conf"]
