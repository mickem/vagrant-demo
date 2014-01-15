# -*- mode: ruby -*-
# vi: set ft=ruby :

# Vagrantfile API/syntax version. Don't touch unless you know what you're doing!
VAGRANTFILE_API_VERSION = "2"

NUM_BOXES = 5

Vagrant.configure(VAGRANTFILE_API_VERSION) do |config|

  config.vm.box = "vagrant-demo"
  config.vm.box_url = "http://files.vagrantup.com/precise64.box"
  config.vm.hostname = "vagrant.temp.medin.name"

  (1..NUM_BOXES).each do |i|
	config.vm.define "slave-#{i}".to_sym do |slave|
      slave.vm.box = "slave-#{i}"
      slave.vm.provider :virtualbox do |vb|
        vb.customize ["modifyvm", :id, "--memory", "1024"]
    	vb.gui = true
      end
    
      slave.vm.synced_folder "scripts", "/home/vagrant/scripts"
      slave.vm.provision "shell", inline: "su - vagrant -c ./scripts/test.sh"
      slave.vm.provision "puppet"
	end
  end
  
  
end
