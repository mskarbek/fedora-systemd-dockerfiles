Systemd based Dockerfiles for Fedora
====================================

## systemd

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

### Configure:

```
cp ~/.ssh/id_rsa.pub openssh-server/authorized_keys
```

### Build:

```
cd openssh-server
sudo docker build --rm=true -t <username>/openssh-server .
```

### Run:

```
sudo docker run --privileged=true -d -P -v /sys/fs/cgroup:/sys/fs/cgroup:ro <username>/openssh-server
```

## nginx

### Configure:

### Build:

```
cd nginx
sudo docker build --rm=true -t <username>/nginx .
```

### Run:

```
sudo docker run --privileged=true -d -P -v /sys/fs/cgroup:/sys/fs/cgroup:ro <username>/nginx
```

## postgresql

### Configure:

### Build:

```
cd postgresql
sudo docker build --rm=true -t <username>/postgresql .
```

### Run:

```
sudo docker run --privileged=true -d -P -v /sys/fs/cgroup:/sys/fs/cgroup:ro <username>/postgresql
```

## bind

### Configure:

### Build:

### Run:

## redis

### Configure:

No extra configuration needed.

### Build:

### Run:

## django

### Configure:

### Build:

### Run:

## docker-repository

### Configure:

### Build:

### Run:

