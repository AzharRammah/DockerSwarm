Vagrant.configure("2") do |config|

  # Configuración general
  config.vm.box = "ubuntu/focal64"

  # Nodo manager de Docker Swarm
  config.vm.define "manager" do |manager|
    manager.vm.hostname = "manager"
    manager.vm.network "private_network", ip: "192.168.50.10"
    manager.vm.provider "virtualbox" do |v|
      v.name = "manager"
    end
    manager.vm.provision "shell", inline: <<-SHELL
      curl -fsSL https://get.docker.com -o get-docker.sh
      sudo sh get-docker.sh
      sudo usermod -aG docker vagrant
      sudo systemctl enable docker
      sudo systemctl start docker
      sudo docker swarm init --advertise-addr 192.168.50.10
    SHELL
  end

  # Nodos worker de Docker Swarm
  (1..3).each do |i|
    config.vm.define "worker#{i}" do |worker|
      worker.vm.hostname = "worker#{i}"
      worker.vm.network "private_network", ip: "192.168.50.#{10 + i}"
      worker.vm.provider "virtualbox" do |v|
        v.name = "worker#{i}"
      end
      worker.vm.provision "shell", inline: <<-SHELL
        curl -fsSL https://get.docker.com -o get-docker.sh
        sudo sh get-docker.sh
        sudo usermod -aG docker vagrant
        sudo systemctl enable docker
        sudo systemctl start docker
      SHELL
    end
  end

end
