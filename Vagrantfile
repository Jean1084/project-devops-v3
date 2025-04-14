# Définition des scripts de setup
SETUP_PROJECT_SCRIPT = "setup/setup_project.sh"
SETUP_MASTER = "setup/setup_master.sh"
SETUP_NODE_1 = "setup/setup_node_1.sh"
SETUP_NODE_2 = "setup/setup_node_2.sh"

Vagrant.configure("2") do |config|
  config.vm.box = "ubuntu/focal64"
  config.vm.synced_folder ".", "/home/vagrant/workspace", type: "virtualbox"
  config.vm.provision "shell", path: SETUP_PROJECT_SCRIPT
  
  config.vm.define "master" do |master_config|
    master_config.vm.network "private_network", type: "static", ip: "192.168.56.10"
    master_config.vm.hostname = "master"
    master_config.vm.provision "shell", path: SETUP_MASTER
    master_config.vm.provider "virtualbox" do |vb|
      vb.name = "vm-master"
      vb.memory = "2048"
      vb.cpus = "2"
    end  
  end

  config.vm.define "node-app" do |node_app_config|
    node_app_config.vm.network "private_network", type: "static", ip: "192.168.56.11"
    node_app_config.vm.hostname = "node-app"
    node_app_config.vm.provision "shell", path: SETUP_NODE_1
    node_app_config.vm.provision "shell", inline: <<-SHELL
      # Copier la clé publique du master vers chaque node_1
      echo "[SSH] Propagation de la cle master transfert de la clé publique du master vers le node_1..."
      ssh master -c "sshpass -p 'vagrant' ssh-copy-id -o StrictHostKeyChecking=accept-new vagrant@192.168.56.11"
    SHELL
    node_app_config.vm.provider "virtualbox" do |vb|
      vb.name = "vm-node-app"
      vb.memory = "1024"
      vb.cpus = "1"
    end  
  end

  config.vm.define "node-monitoring" do |node_monitoring_config|
    node_monitoring_config.vm.network "private_network", type: "static", ip: "192.168.56.12"
    node_monitoring_config.vm.hostname = "node-monitoring"
    node_monitoring_config.vm.provision "shell", path: SETUP_NODE_2
    node_monitoring_config.vm.provision "shell", inline: <<-SHELL
      # Copier la clé publique du master vers chaque node_2
      echo "[SSH] Propagation de la cle master transfert de la clé publique du master vers le node_2..."
      ssh master -c "sshpass -p 'vagrant' ssh-copy-id -o StrictHostKeyChecking=accept-new vagrant@192.168.56.12"
    SHELL
    node_monitoring_config.vm.provider "virtualbox" do |vb|
      vb.name = "vm-node-monitoring"
      vb.memory = "1024"
      vb.cpus = "1"
    end  
  end
end