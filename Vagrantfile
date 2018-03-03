# -*- mode: ruby -*-
# vi: set ft=ruby :


# http://stackoverflow.com/questions/19492738/demand-a-vagrant-plugin-within-the-vagrantfile
# not using 'vagrant-vbguest' vagrant plugin because now using bento images which has vbguestadditions preinstalled.
required_plugins = %w( vagrant-hosts vagrant-share vagrant-vbox-snapshot vagrant-host-shell vagrant-triggers vagrant-reload )
plugins_to_install = required_plugins.select { |plugin| not Vagrant.has_plugin? plugin }
if not plugins_to_install.empty?
  puts "Installing plugins: #{plugins_to_install.join(' ')}"
  if system "vagrant plugin install #{plugins_to_install.join(' ')}"
    exec "vagrant #{ARGV.join(' ')}"
  else
    abort "Installation of one or more plugins has failed. Aborting."
  end
end



Vagrant.configure(2) do |config|
  config.vm.define "webserver-box" do |webserver_config|
    webserver_config.vm.box = "bento/centos-7.4"
    webserver_config.vm.hostname = "webserver.local"
    # https://www.vagrantup.com/docs/virtualbox/networking.html
    webserver_config.vm.network "private_network", ip: "192.168.50.10"
    webserver_config.vm.network "private_network", ip: "10.0.0.10", :netmask => "255.255.255.0", virtualbox__intnet: "intnet2"

    webserver_config.vm.provider "virtualbox" do |vb|
      vb.gui = true
      vb.memory = "1024"
      vb.cpus = 2
      vb.customize ["modifyvm", :id, "--clipboard", "bidirectional"]
      vb.name = "centos7_webserver"
    end

    webserver_config.vm.provision "shell", path: "scripts/install-rpms.sh", privileged: true
  end


  config.vm.define "box1" do |box1_config|
    box1_config.vm.box = "bento/centos-7.4"
    box1_config.vm.hostname = "box1.local"
    box1_config.vm.network "private_network", ip: "10.0.0.11", :netmask => "255.255.255.0", virtualbox__intnet: "intnet2"

    box1_config.vm.provider "virtualbox" do |vb|
      vb.gui = true
      vb.memory = "1024"
      vb.cpus = 2
      vb.name = "centos7_box1"
    end

    box1_config.vm.provision "shell", path: "scripts/install-rpms.sh", privileged: true
  end


  config.vm.define "box2" do |box2_config|
    box2_config.vm.box = "bento/centos-7.3"
    box2_config.vm.hostname = "box2.local"
    box2_config.vm.network "private_network", ip: "10.0.0.12", :netmask => "255.255.255.0", virtualbox__intnet: "intnet2"

    box2_config.vm.provider "virtualbox" do |vb|
      vb.gui = true
      vb.memory = "1024"
      vb.cpus = 2
      vb.name = "centos7_box2"
    end

    box2_config.vm.provision "shell", path: "scripts/install-rpms.sh", privileged: true
  end
end
