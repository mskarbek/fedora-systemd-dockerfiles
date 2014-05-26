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

RUN yum -y install postgresql-server postgresql-contrib; yum clean all; \
systemctl enable postgresql.service; \
su - postgres -c 'pg_ctl initdb'; \
su - postgres -c '/usr/bin/pg_ctl -D /var/lib/pgsql/data start' && sleep 10; \
su - postgres -c 'createuser dockeruser'; \
su - postgres -c 'createdb dockerdb'; \
su - postgres -c "psql -c \"ALTER ROLE dockeruser WITH PASSWORD 'password';\""; \
su - postgres -c '/usr/bin/pg_ctl -D /var/lib/pgsql/data stop';

ADD pg_hba.conf /var/lib/pgsql/data/pg_hba.conf
ADD postgresql.conf /var/lib/pgsql/data/postgresql.conf

RUN chown postgres:postgres /var/lib/pgsql/data/pg_hba.conf /var/lib/pgsql/data/postgresql.conf;

EXPOSE 5432

VOLUME ["/sys/fs/cgroup"]
CMD ["/usr/sbin/init"]
