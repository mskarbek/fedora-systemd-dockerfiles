FROM fedora
MAINTAINER "Marcin Skarbek" <docker@skarbek.name>

ENV container docker

RUN yum -y update; yum clean all;

RUN yum -y install systemd; yum clean all; \
(cd /lib/systemd/system/sysinit.target.wants/; for i in *; do [ $i == systemd-tmpfiles-setup.service ] || rm -f $i; done); \
rm -f /lib/systemd/system/multi-user.target.wants/*; \
rm -f /etc/systemd/system/*.wants/*; \
rm -f /lib/systemd/system/local-fs.target.wants/*; \
rm -f /lib/systemd/system/sockets.target.wants/*udev*; \
rm -f /lib/systemd/system/sockets.target.wants/*initctl*; \
rm -f /lib/systemd/system/basic.target.wants/*; \
rm -f /lib/systemd/system/anaconda.target.wants/*;

RUN yum -y install python-pip; \
pip install django gunicorn; \
cd /srv; django-admin.py startproject djdocker; \
useradd djdocker; \
chown -R djdocker:djdocker /srv/djdocker;

ADD djdocker.service /etc/systemd/system/djdocker.service

RUN systemctl enable djdocker.service;

EXPOSE 8000

VOLUME ["/sys/fs/cgroup"]
CMD ["/usr/sbin/init"]
