odoo-all-in-one
===========

This is an image that allows you to launch the odoo v8 container with an already configured PostgreSQL database in the same container

Configuration
-----------

Launch the container with:

    sudo docker run --rm -ti -p 8069:8069 adhoc/odoo-all-in-one /bin/bash

the --rm will remove the container once it is stopped.

Once in the container launch:

    service postgresql start && sudo -H -u odoo /opt/odoo/server/odoo.py \
        -c /opt/odoo/odoo.conf

this will launch the PostgreSQL server and after the Odoo instance.

If you want to maintain the information of the database the container should be launched with aditional volumes parameters like this:

    sudo docker run --rm -ti -p 8069:8069 \
        -v /your/local/data/directory:/var/lib/postgresql/data \
        -v /your/local/log/directory:/var/log/postgresql postgres:9.3 \
        adhoc/odoo-all-in-one /bin/bash

Development
-----------

If you want to use this docker image for development, you can launch it using:

    sudo docker run --rm -ti -p 8069:8069 \
        -v /your/local/repo:/opt/odoo/sources/addons \
        adhoc/odoo-all-in-one /bin/bash

where /your/local/repo is your currently cloned repository of some module. This will allow you to change the code of the module locally without the need of creating an image every time you modify the module.
