# -*- mode: ruby -*-
# vi: set ft=ruby :
require 'yaml'

yaml = YAML.load_file("machines.yml")

Vagrant.configure("2") do |config|
  yaml.each do |server|

    config.trigger.before :up do |before|
      before.ruby do |env,machine|
        File.open('.hosts.tmp', 'a') do |hosts|
          hosts.write("127.0.0.1 localhost \n#{server['ip']} #{server['hostname']} \n")
        File.open("hosts", "w+") { |file| file.puts File.readlines(".hosts.tmp").uniq }
        end
        if !(File.exists?('id_rsa'))
          system("ssh-keygen -b 2048 -t rsa -f id_rsa -q -N ''")
        end
      end
    end

    config.vm.define server["name"] do |srv|
      srv.vm.box = server["system"]
      srv.vm.network "private_network", ip: server["ip"]
      srv.vm.hostname = server["hostname"]
      srv.vm.provider "virtualbox" do |vb|
        vb.name = server["name"]
        vb.memory = server["memory"]
        vb.cpus = server["cpus"]
      end

      if server["system"] == "ubuntu/bionic64"
        srv.vm.provision "shell", inline: "apt-get update; apt-get install python python-pip python3 python3-pip -y; apt-get update; apt-get install -y software-properties-common; apt-add-repository --yes --update ppa:ansible/ansible; apt-get install -y ansible;"
      end

      if server["system"] == "centos/7"
        srv.vm.provision "shell", inline:  "yum update; yum install epel-release python2 python2-pip python3 python3-pip policycoreutils-python git wget vim -y; yum install -y ansible;"
      end

      if server["name"] == "4flix"
        srv.vm.provision "shell", inline: <<-SHELL
    yum update;
    yum install unzip java-1.8.0-openjdk yum-utils device-mapper-persistent-data lvm2 -y
    curl -O http://ftp.unicamp.br/pub/apache//jmeter/binaries/apache-jmeter-5.2.zip
    unzip apache-jmeter-5.2.zip
    mv apache-jmeter-5.2/ /root
    yum-config-manager --add-repo https://download.docker.com/linux/centos/docker-ce.repo
    wget -O /etc/yum.repos.d/jenkins.repo http://pkg.jenkins-ci.org/redhat-stable/jenkins.repo
    rpm --import https://jenkins-ci.org/redhat/jenkins-ci.org.key
    echo "[mongodb-org-3.6]
name=MongoDB Repository
baseurl=https://repo.mongodb.org/yum/redhat/7/mongodb-org/3.6/x86_64/
gpgcheck=1
enabled=1
gpgkey=https://www.mongodb.org/static/pgp/server-3.6.asc" > /etc/yum.repos.d/mongodb-org-3.6.repo
    yum install docker-ce jenkins mongodb-org-shell.x86_64 -y
    echo "jenkins ALL=(ALL) NOPASSWD:ALL" >> /etc/sudoers
    systemctl enable jenkins docker
    systemctl start jenkins docker
    SHELL
    end

      config.vm.provision "shell", inline: "cp /vagrant/hosts /etc/hosts"
      config.vm.provision "shell", inline: "mkdir -p /root/.ssh"
      config.vm.provision "shell", inline: "cp /vagrant/id_rsa /root/.ssh/id_rsa"
      config.vm.provision "shell", inline: "cp /vagrant/id_rsa.pub /root/.ssh/authorized_keys"
      config.vm.provision "shell", inline: "chmod 600 /root/.ssh/id_rsa"

    end
  end
end   
