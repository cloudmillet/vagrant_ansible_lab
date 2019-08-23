# -*- mode: ruby -*-
# vi: set ft=ruby :

$commonscript = <<-SCRIPT
sudo yum update -y
sudo yum install python2 epel-release -y
sudo echo "192.168.22.10	ansiblecontroller.example.com ansiblecontroller" >> /etc/hosts
sudo echo "192.168.22.11   node01.example.com 	node01" >> /etc/hosts
sudo echo "192.168.22.12   node02.example.com      node02" >> /etc/hosts
sudo echo "192.168.22.13   node03.example.com      node03" >> /etc/hosts
sudo echo "LANG=en_US.utf-8" > /etc/environment
sudo echo "LC_ALL=en_US.utf-8" >> /etc/environment
SCRIPT

$nodescript = <<-SCRIPT
cat /vagrant/certificates/ansible_lab.pub >> /home/vagrant/.ssh/authorized_keys
SCRIPT

$ansiblescript = <<-SCRIPT
sudo yum install ansible -y
sudo cp -r /vagrant/certificates/ansible_lab /home/vagrant/.ssh/id_rsa
sudo chmod 400  /home/vagrant/.ssh/id_rsa
sudo chown vagrant:vagrant /home/vagrant/.ssh/id_rsa
SCRIPT

Vagrant.configure("2") do |config|

    config.vm.define "ansiblecontroller" do |ansiblecontroller|

         # Box
         ansiblecontroller.vm.box = "centos/7"

         # Memory and CPU allocation
         ansiblecontroller.vm.provider "virtualbox" do |v|
            v.memory = 512
            v.cpus = 1
         end

         # IP allocation
         ansiblecontroller.vm.network "private_network", ip: "192.168.22.10", virtualbox__intnet: "mynetwork01"

         # Host name allocation
         ansiblecontroller.vm.hostname = "ansiblecontroller.example.com"

         # Installing required packages for ansible controller node
         ansiblecontroller.vm.provision "shell", inline: $commonscript
         ansiblecontroller.vm.provision "shell", inline: $ansiblescript
     end

     # https://www.vagrantup.com/docs/vagrantfile/tips.html
     (1..3).each do |i|
         config.vm.define "node0#{i}" do |node|

          # Box
          node.vm.box = "centos/7"

          # Memory and CPU allocation
          node.vm.provider "virtualbox" do |v|
              v.memory = 512
              v.cpus = 1
          end

          node.vm.network "private_network", ip: "192.168.22.1#{i}", virtualbox__intnet: "mynetwork01"

          node.vm.hostname = "node0#{i}.example.com"

          # Installing required packages for  node01
          node.vm.provision "shell", inline: $commonscript
          node.vm.provision "shell", inline: $nodescript
         end
      end
end
