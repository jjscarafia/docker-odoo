FROM ubuntu:14.04
MAINTAINER Damian Soriano <ds@ingadhoc.com>

ENV REFRESHED_AT 2014-08-30

RUN apt-get clean
RUN apt-get update
RUN apt-get upgrade -y

RUN apt-get update && apt-get install -y git
RUN apt-get update && apt-get install -y python-pip

RUN useradd -m -p odoo odoo

RUN mkdir -p /opt/odoo
RUN git clone https://github.com/odoo/odoo.git /opt/odoo/server

RUN chown -R odoo.odoo /opt/odoo/
