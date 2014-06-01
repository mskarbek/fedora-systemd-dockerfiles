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

RUN yum -y install bind bind-utils; yum clean all; \
systemctl enable named.service; \
rndc-confgen -a -c /etc/rndc.key; \
chown named:named /etc/rndc.key;

ADD rndc-reload.timer /etc/systemd/system/rndc-reload.timer
ADD rndc-reload.service /etc/systemd/system/rndc-reload.service
ADD rndc-reload /usr/local/bin/rndc-reload
ADD named.conf /etc/named/named.conf

RUN systemctl enable rndc-reload.timer; \
rm -f /etc/named.conf; \
ln -s /etc/named/named.conf /etc/named.conf; \
mkdir /var/log/named; \
chown named:named /var/log/named;

EXPOSE 53

VOLUME ["/sys/fs/cgroup", "/etc/named", "/var/log/named"]
CMD ["/usr/sbin/init"]
