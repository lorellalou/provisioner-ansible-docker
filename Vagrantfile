# -*- mode: ruby -*-
# vi: set ft=ruby :

VAGRANTFILE_API_VERSION = "2"

Vagrant.configure(VAGRANTFILE_API_VERSION) do |config|
  config.vm.box = "webhippie/opensuse-13.2"
  config.vm.provider "virtualbox" do |v|
    v.memory = 2048
  end
  config.vm.network :forwarded_port, host: 2222, guest: 22, id: "ssh", auto_correct: true
  config.vm.network "private_network", ip: "192.168.50.80"
config.vm.provision "shell", inline: <<-SCRIPT
    if ! type docker >/dev/null; then
      echo -e "\n\n========= installing docker..."
      curl -sL https://get.docker.io/ | sh
      service docker start
      echo -e "\n\n========= installing docker bash completion..."
      curl -sL https://raw.githubusercontent.com/dotcloud/docker/master/contrib/completion/bash/docker > /etc/bash_completion.d/docker
      usermod -G docker vagrant
    fi
    if ! type pip >/dev/null; then
      echo -e "\n\n========= installing pip..."
      curl -sk https://bootstrap.pypa.io/get-pip.py | python  
    fi
    if ! type docker-compose >/dev/null; then
      echo -e "\n\n========= installing docker-compose..."
      pip install -U docker-compose
      echo -e "\n\n========= installing docker-compose command completion..."
      curl -sL https://raw.githubusercontent.com/docker/compose/$(docker-compose --version | awk 'NR==1{print $NF}')/contrib/completion/bash/docker-compose > /etc/bash_completion.d/docker-compose
    fi
SCRIPT
  config.vm.hostname = "suse-provisioner"
  config.vm.synced_folder ".", "/vagrant"  
  if Vagrant.has_plugin?("vagrant-cachier")
    config.cache.scope = :box
  end
end
