# -*- mode: ruby -*-
# vi: set ft=ruby :

# Vagrantfile API/syntax version. Don't touch unless you know what you're doing!
VAGRANTFILE_API_VERSION = "2"

Vagrant.configure(VAGRANTFILE_API_VERSION) do |config|

  config.vm.box = "vagrant-demo"
  config.vm.box_url = "http://files.vagrantup.com/precise64.box"
  config.vm.hostname = "vagrant.temp.medin.name"

  config.vm.provider :virtualbox do |vb|
    vb.customize ["modifyvm", :id, "--memory", "4096"]
	vb.gui = true
  end

  config.vm.synced_folder "scripts", "/home/vagrant/scripts"
  config.vm.provision "shell", inline: "su - vagrant -c ./scripts/test.sh"
  config.vm.provision "shell", inline: "/home/vagrant/scripts/provision.sh"

end
