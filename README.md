# Mesos in Docker in Vagrant

## Requirements

* [Vagrant](https://www.vagrantup.com/)
* [Docker](https://www.docker.com/)

### Fedora

```bash
yum install -y vagrant docker-io
```

**Note:** If you use BTRFS, you need to disable SELinux to be able to run docker.
```bash
sed -i s/SELINUX=enforcing/SELINUX=disabled/g /etc/selinux/config
```
## Docker containers

The following containers are used and need to be pulled:
* [jplock/zookeeper](https://registry.hub.docker.com/u/jplock/zookeeper/)
* [redjack/mesos-master](https://registry.hub.docker.com/u/redjack/mesos-master/)
* [redjack/mesos-slave](https://registry.hub.docker.com/u/redjack/mesos-slave/)
* [mesosphere/marathon](https://registry.hub.docker.com/u/mesosphere/marathon/)

```bash
docker pull jplock/zookeeper:3.4.6
docker pull redjack/mesos-master
docker pull redjack/mesos-slave
docker pull mesosphere/marathon
```

## Usage

### Start all the things

```bash
vagrant up zookeeper --provider=docker
vagrant up mesos-master --provider=docker
vagrant up mesos-slave --provider=docker
vagrant up marathon --provider=docker
```

### Access Mesos UI

Go to [127.0.0.1:5050](http://127.0.0.1:5050/).

To be able to use all the features of the UI, you need to be able to resolve the domain name of the slave.
It can be done by adding `127.0.0.1   mesos-slave` to your `/etc/hosts` file.

### Access Marathon UI

Go to [127.0.0.1:8080](http://127.0.0.1:8080/).

