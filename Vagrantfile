# -*- mode: ruby -*-
# vi: set ft=ruby :
require 'yaml'
yaml = YAML.load_file("machines.yml")

#if !(File.exists?('id_rsa'))
#  exec "ssh-keygen -b 2048 -t rsa -f id_rsa -q -N ''"
#end

#      if !(File.zero?('hosts'))
#        File.open('hosts', 'a') do |hosts|
#          hosts.write("#{server['hostname']} #{server['ip']}\n")
#        end
#      end

#    config.trigger.after :up do |ssh|
#      if !(File.exists?('id_rsa'))
#        exec "ssh-keygen -b 2048 -t rsa -f id_rsa -q -N ''"
#      end
#    end

Vagrant.configure("2") do |config|
yaml.each do |server|

    config.trigger.before :up do |trigger|
      #trigger.run_remote = {inline: "echo #{server['ip']} #{server['hostname']}\n > /tmp/teste"}
      trigger.ruby do |env,machine|
        if !(File.exists?('id_rsa'))
          exec "ssh-keygen -b 2048 -t rsa -f id_rsa -q -N ''"
        end
        if !(File.zero?('hosts'))
          File.open('hosts', 'a') do |hosts|
            hosts.write("#{server['ip']} #{server['hostname']} \n")
          end
        end
      end
    end

    config.vm.define server["name"] do |srv|
      srv.vm.box = server["sistema"]
      srv.vm.network "private_network", ip: server["ip"]
      srv.vm.hostname = server["hostname"]
      srv.vm.provider "virtualbox" do |vb|
        vb.name = server["name"]
        vb.memory = server["memory"]
        vb.cpus = server["cpus"]
      end

      if server["sistema"] == "ubuntu/bionic64"
        srv.vm.provision "shell", inline: "apt install python -y"
      end

#      config.vm.provision "shell", inline: "mkdir -p /root/.ssh"
#      config.vm.provision "shell", inline: "cp /vagrant/id_rsa /root/.ssh/id_rsa"
#      config.vm.provision "shell", inline: "cp /vagrant/id_rsa.pub /root/.ssh/authorized_keys"
#      config.vm.provision "shell", inline: "chmod 600 /root/.ssh/id_rsa"
#      config.vm.provision "shell", inline: "cat /vagrant/hosts | uniq >> /etc/hosts"

#      srv.vm.provision :ansible do |ansible|
#        ansible.limit = "all"
#        ansible.compatibility_mode = "2.0"
#        ansible.playbook = "playbook.yml"
#      end
    end
  end
end

