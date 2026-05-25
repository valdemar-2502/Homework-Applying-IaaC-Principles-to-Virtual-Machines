# -*- mode: ruby -*-
# vi: set ft=ruby :

ISO = "bento/ubuntu-20.04"
NET = "192.168.56."
DOMAIN = ".netology"
HOST_PREFIX = "server"

servers = [
  {
    :hostname => HOST_PREFIX + "1" + DOMAIN,
    :ip => NET + "11",
    :ssh_host => 20011,
    :ssh_vm => 22,
    :ram => 1024,
    :core => 1
  }
]

Vagrant.configure("2") do |config|
  config.vm.synced_folder ".", "/vagrant", disabled: false

  servers.each do |machine|
    config.vm.define machine[:hostname] do |node|
      node.vm.box = ISO
      node.vm.hostname = machine[:hostname]
      node.vm.network "private_network", ip: machine[:ip]
      node.vm.network :forwarded_port, guest: machine[:ssh_vm], host: machine[:ssh_host]

      node.vm.provider "virtualbox" do |vb|
        vb.customize ["modifyvm", :id, "--memory", machine[:ram]]
        vb.customize ["modifyvm", :id, "--cpus", machine[:core]]
        vb.name = machine[:hostname]
      end

      node.vm.provision "shell", inline: <<-SHELL
        export DEBIAN_FRONTEND=noninteractive

        # Update system
        sudo apt-get update

        # Install prerequisites
        sudo apt-get install -y ca-certificates curl gnupg lsb-release

        # Add Docker's official GPG key
        sudo install -m 0755 -d /etc/apt/keyrings
        curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo gpg --dearmor -o /etc/apt/keyrings/docker.gpg
        sudo chmod a+r /etc/apt/keyrings/docker.gpg

        # Add Docker repository
        echo \
          "deb [arch=$(dpkg --print-architecture) signed-by=/etc/apt/keyrings/docker.gpg] https://download.docker.com/linux/ubuntu \
          $(. /etc/os-release && echo "$VERSION_CODENAME") stable" | \
          sudo tee /etc/apt/sources.list.d/docker.list > /dev/null

        # Install Docker
        sudo apt-get update
        sudo apt-get install -y docker-ce docker-ce-cli containerd.io docker-buildx-plugin docker-compose-plugin

        # Add vagrant user to docker group
        sudo usermod -aG docker vagrant

        echo "========================================="
        echo "Docker installed successfully!"
        echo "========================================="
        docker --version
        docker compose version
      SHELL
    end
  end
end
