Vagrant.configure("2") do |config|
  config.vm.box = "wheezy64Perso"
  config.vm.box_url = "http://puppet-vagrant-boxes.puppetlabs.com/debian-70rc1-x64-vbox4210.box"

  config.vm.network :private_network, ip: "192.168.56.101"
    config.vm.network :forwarded_port, guest: 80, host: 8080
    config.ssh.forward_agent = true

  config.vm.provider :virtualbox do |v|
    v.customize ["modifyvm", :id, "--natdnshostresolver1", "on"]
    v.customize ["modifyvm", :id, "--memory", 512]
    v.customize ["modifyvm", :id, "--name", "perso"]
  end

  
  config.vm.synced_folder "./Projects", "/var/www", id: "vagrant-root" 
  config.vm.synced_folder "../../Dropbox", "/var/dropbox", id: "dropbox" 
  config.vm.provision :shell, :inline =>
    "if [[ ! -f /apt-get-run ]]; then sudo apt-get update && sudo touch /apt-get-run; fi"


  config.vm.provision :shell, :inline => 'echo -e "mysql_root_password=mypass
controluser_password=awesome" > /etc/phpmyadmin.facts;'

  config.vm.provision :puppet do |puppet|
    puppet.manifests_path = "manifests"
    puppet.module_path = "modules"
    puppet.options = ['--verbose']
  end
end
