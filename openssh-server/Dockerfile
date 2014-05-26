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

RUN yum -y install openssh-server; yum clean all; \
systemctl enable sshd; \
useradd -G wheel user;

ADD authorized_keys /home/user/.ssh/authorized_keys
ADD sudoers /etc/sudoers
ADD sshd /etc/pam.d/sshd

RUN chown -R user:user /home/user/.ssh; \
chmod 700 /home/user/.ssh; \
chmod 600 /home/user/.ssh/authorized_keys;

EXPOSE 22

VOLUME ["/sys/fs/cgroup"]
CMD ["/usr/sbin/init"]
