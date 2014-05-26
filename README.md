Systemd based Dockerfiles for Fedora
====================================

## systemd

Creates basic systemd image, all other images in this repo are based on this Dockerfile.
Dockerfile from [Daniel Walsh blog](http://rhatdan.wordpress.com/2014/04/30/running-systemd-within-a-docker-container/).

### Configure:

No extra configuration needed.

### Build:

```
cd systemd
sudo docker build --rm=true -t <username>/systemd .
```

### Run:

```
sudo docker run --privileged=true -d -v /sys/fs/cgroup:/sys/fs/cgroup:ro <username>/systemd
```

## openssh-server

Image with one user `user` added to group `wheel`. Sudo configured with NOPASSWD for `wheel` group.
No password for user set.

### Configure:

```
cp ~/.ssh/id_rsa.pub openssh-server/authorized_keys
```

If you don't need sudo enabled user remove `-G wheel` from line 20 in [openssh-server/Dockerfile](https://github.com/mskarbek/fedora-systemd-dockerfiles/blob/master/openssh-server/Dockerfile#L20).

### Build:

```
cd openssh-server
sudo docker build --rm=true -t <username>/openssh-server .
```

### Run:

For random port binding:

```
sudo docker run --privileged=true -d -P -v /sys/fs/cgroup:/sys/fs/cgroup:ro <username>/openssh-server
```

For specific port binding:

```
sudo docker run --privileged=true -d -p <port>:22 -v /sys/fs/cgroup:/sys/fs/cgroup:ro <username>/openssh-server
```

### Test:

```
ssh -p <port> user@localhost
```

## nginx

Basic nginx image.

### Configure:

No extra configuration needed.

### Build:

```
cd nginx
sudo docker build --rm=true -t <username>/nginx .
```

### Run:

For random port binding:

```
sudo docker run --privileged=true -d -P -v /sys/fs/cgroup:/sys/fs/cgroup:ro <username>/nginx
```

For specific port binding:

```
sudo docker run --privileged=true -d -p <port>:80 -v /sys/fs/cgroup:/sys/fs/cgroup:ro <username>/nginx
```

### Test:

```
curl localhost:<port>
```

## postgresql

PostgreSQL configured with one user `dockeruser`, password `password` and database `dockerdb`.

### Configure:

If you need custom user/database name and/or password edit lines 22-24 in [openssh-server/Dockerfile](https://github.com/mskarbek/fedora-systemd-dockerfiles/blob/master/postgresql/Dockerfile#L22-L24).

### Build:

```
cd postgresql
sudo docker build --rm=true -t <username>/postgresql .
```

### Run:

For random port binding:

```
sudo docker run --privileged=true -d -P -v /sys/fs/cgroup:/sys/fs/cgroup:ro <username>/postgresql
```

For specific port binding:

```
sudo docker run --privileged=true -d -p <port>:5432 -v /sys/fs/cgroup:/sys/fs/cgroup:ro <username>/postgresql
```

### Test:

```
psql -h localhost -p <port> -U dockeruser dockerdb
```

## bind


### Configure:

```
sudo mkdir -p /srv/bind/{etc,log}
sudo chown -R named:named /srv/bind
cp bind/named.conf /srv/bind/etc
```

Additional zone files copy to `/srv/bind/etc`.
Configuration i reloaded hourly as set in [timer-hourly.timer](https://github.com/mskarbek/fedora-systemd-dockerfiles/blob/master/bind/timer-hourly.timer) using [rndc-reload.service](https://github.com/mskarbek/fedora-systemd-dockerfiles/blob/master/bind/rndc-reload.service).

### Build:

```
cd bind
sudo docker build --rm=true -t <username>/bind .
```

### Run:

```
sudo docker run --privileged=true -d -v /sys/fs/cgroup:/sys/fs/cgroup:ro -v /srv/bind/etc:/etc/named:rw -v /srv/bind/log:/var/log/named:rw -p 127.0.0.1:53:53/udp -p 127.0.0.1:53:53/tcp <username>/bind
```

### Test:

```
dig google.com @localhost
```

## redis

Basic redis image.

### Configure:

No extra configuration needed.

### Build:

```
cd redis
sudo docker build --rm=true -t <username>/redis .
```

### Run:

For random port binding:

```
sudo docker run --privileged=true -d -P -v /sys/fs/cgroup:/sys/fs/cgroup:ro <username>/redis
```

For specific port binding:

```
sudo docker run --privileged=true -d -p <port>:6379 -v /sys/fs/cgroup:/sys/fs/cgroup:ro <username>/redis
```

### Test:

```
nc localhost <port>
```

## django

Basic django image. Gunicorn as a server.

### Configure:

No extra configuration needed.

### Build:

```
cd django
sudo docker build --rm=true -t <username>/django .
```

### Run:

For random port binding:

```
sudo docker run --privileged=true -d -P -v /sys/fs/cgroup:/sys/fs/cgroup:ro <username>/django
```

For specific port binding:

```
sudo docker run --privileged=true -d -p <port>:8000 -v /sys/fs/cgroup:/sys/fs/cgroup:ro <username>/django
```

### Test:

```
curl localhost:<port>
```

## docker-repository

### Configure:

### Build:

### Run:
