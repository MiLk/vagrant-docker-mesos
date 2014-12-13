# -*- mode: ruby -*-
# vi: set ft=ruby :

# Vagrantfile API/syntax version. Don't touch unless you know what you're doing!
VAGRANTFILE_API_VERSION = "2"
ENV['VAGRANT_DEFAULT_PROVIDER'] = 'docker'

Vagrant.configure(VAGRANTFILE_API_VERSION) do |config|
  
  config.vm.define "zookeeper" do |base|
    base.vm.provider "docker" do |d|
      d.name = "zookeeper"
      d.image = "jplock/zookeeper:3.4.6"
      d.remains_running = "true"
      d.ports = ["2181:2181"]
    end
  end

  config.vm.define "mesos-master" do |base|
    base.vm.synced_folder "logs/mesos-master/", "/var/log"
    base.vm.provider "docker" do |d|
      d.name = "mesos-master"
      d.image = "redjack/mesos-master"
      d.remains_running = "true"
      d.env = {
        "MESOS_LOG_DIR" => "/var/log",
        "MESOS_WORK_DIR" => "/var/lib/mesos",
        "MESOS_QUORUM" => 1,
        "MESOS_ZK" => "zk://zookeeper:2181/mesos",
        "MESOS_HOSTNAME" => "mesos-master"
      }
      d.ports = ["5050:5050"]
      d.link("zookeeper:zookeeper")
    end
  end

  config.vm.define "mesos-slave" do |base|
    base.vm.synced_folder "logs/mesos-slave/", "/var/log"
    base.vm.provider "docker" do |d|
      d.name = "mesos-slave"
      d.image = "redjack/mesos-slave"
      d.remains_running = "true"
      d.env = {
        "MESOS_LOG_DIR" => "/var/log",
        "MESOS_MASTER" => "zk://zookeeper:2181/mesos",
        "MESOS_HOSTNAME" => "mesos-slave"
      }
      d.ports = ["5051:5051"]
      d.link("zookeeper:zookeeper")
    end
  end

  config.vm.define "marathon" do |base|
    base.vm.provider "docker" do |d|
      d.name = "marathon"
      d.image = "mesosphere/marathon"
      d.remains_running = "true"
      d.ports = ["8080:8080"]
      d.link("zookeeper:zookeeper")
      d.cmd = [
               "--master", "zk://zookeeper:2181/mesos",
               "--zk", "zk://zookeeper:2181/marathon"
              ]
    end
  end

end
