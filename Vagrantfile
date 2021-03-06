# -*- mode: ruby -*-
# vi: set ft=ruby :


# https://github.com/hashicorp/vagrant/issues/1874#issuecomment-165904024
# not using 'vagrant-vbguest' vagrant plugin because now using bento images which has vbguestadditions preinstalled.
def ensure_plugins(plugins)
  logger = Vagrant::UI::Colored.new
  result = false
  plugins.each do |p|
    pm = Vagrant::Plugin::Manager.new(
      Vagrant::Plugin::Manager.user_plugins_file
    )
    plugin_hash = pm.installed_plugins
    next if plugin_hash.has_key?(p)
    result = true
    logger.warn("Installing plugin #{p}")
    pm.install_plugin(p)
  end
  if result
    logger.warn('Re-run vagrant up now that plugins are installed')
    exit
  end
end

required_plugins = ['vagrant-hosts', 'vagrant-share', 'vagrant-vbox-snapshot', 'vagrant-host-shell', 'vagrant-reload']
ensure_plugins required_plugins



Vagrant.configure(2) do |config|
  config.vm.define "webserver-box" do |webserver_config|
    webserver_config.vm.box = "bento/centos-7.4"
    webserver_config.vm.hostname = "webserver.local"
    # https://www.vagrantup.com/docs/virtualbox/networking.html
    webserver_config.vm.network "private_network", ip: "10.0.5.10", :netmask => "255.255.255.0", virtualbox__intnet: "intnet2"

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
    box1_config.vm.network "private_network", ip: "10.0.5.11", :netmask => "255.255.255.0", virtualbox__intnet: "intnet2"

    box1_config.vm.provider "virtualbox" do |vb|
      vb.gui = true
      vb.memory = "1024"
      vb.cpus = 2
      vb.name = "centos7_box1"
    end

    box1_config.vm.provision "shell", path: "scripts/install-rpms.sh", privileged: true
  end
end