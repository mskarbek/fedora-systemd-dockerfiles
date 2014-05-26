Systemd based Dockerfiles for Fedora
====================================

## systemd

Basic Dockerfile providing only working systemd, all other Dockerfiles in this repo are based on it.

## openssh-server

To build image you need to add public ssh key to `openssh-server/authorized_keys`:

```
cp ~/.ssh/id_rsa.pub openssh-server/authorized_keys
```

## nginx

## postgresql

## bind

## redis

## django

## docker-repository
