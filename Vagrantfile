# -*- mode: ruby -*-
# vi: set ft=ruby :


if ! ENV['EVB_SKIP_LOGO'] then
	puts "\033[38;5;33m       _______  _____  "
	puts "  ___ <  / __ \\/__  /  "
	puts " / _ \\/ / / / /  / /   " 
	puts "/  __/ / /_/ /  / /    "
	puts "\\___/_/\\____/  /_/     \033[0m"
	puts "\033[38;5;196m _    __                             __  ____              "
	puts "| |  / /___ _____ __________ _____  / /_/ __ )____  _  __  "
	puts "| | / / __ `/ __ `/ ___/ __ `/ __ \\/ __/ __  / __ \\| |/_/  "
	puts "| |/ / /_/ / /_/ / /  / /_/ / / / / /_/ /_/ / /_/ />  <    "
	puts "|___/\\__,_/\\__, /_/   \\__,_/_/ /_/\\__/_____/\\____/_/|_|    "
	puts "          /____/                                                "
	puts "\033[0m"
end

Vagrant.configure "2" do |config|
  config.vm.box = "ubuntu/xenial64"
  #config.vm.box_url = "https://atlas.hashicorp.com/ubuntu/boxes/xenial64/versions/20170516.0.0/providers/virtualbox.box"
  config.vm.box_url = "https://cloud-images.ubuntu.com/xenial/current/xenial-server-cloudimg-amd64-vagrant.box"
  config.vm.define "e107vagrantbox" do |e107vagrantbox|
  end

  
config.vm.provider "virtualbox" do |vb|
  	vb.name = "e107vagrantbox"
    vb.customize ["modifyvm", :id, "--memory", 1024]
    vb.customize ["modifyvm", :id, "--cpuexecutioncap", "50"]
  end

  config.vm.hostname = "ubuntu-xenial64-e107vagrant.box"
  config.vm.network :private_network, {
    ip: "10.0.0.7"
  }

  config.vm.boot_timeout = 500

  config.vm.post_up_message = "Virtual machine set up is now complete. You can visit the IP 10.0.0.7 in your browser to complete e107 installation. To ssh to this virtual machine you can enter 'vagrant ssh' in your terminal, to shutdown use 'vagrant halt', to restart use 'vagrant reload' and to destroy the VM use 'vagrant destroy'. For help enter 'vagrant -h'"

  config.vm.synced_folder "./www/e107dev.box", "/var/www/e107dev.box", create: true, group: "www-data", owner: "www-data"

  config.vm.synced_folder "./logs", "/var/www/logs", create: true, group: "www-data", owner: "www-data"

  
  # Add the tty fix as mentioned in issue 1673 on vagrant repo
  # To avoid 'stdin is not a tty' messages
  # vagrant provisioning in shell runs bash -l
  config.vm.provision "fix-no-tty", type: "shell" do |s|
      s.privileged = false
      s.inline = "sudo sed -i '/tty/!s/mesg n/tty -s \\&\\& mesg n/' /root/.profile"
  end

  config.vm.provision :shell, :path => "provisioner.sh"


  # Service startups (always run)
  config.vm.provision :shell, {
    path: "initializer.sh",
    name: "initializer",
    run: "always"
  }

end
