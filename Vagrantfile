# Définition des scripts de setup
SETUP_PROJECT_SCRIPT = "setup/setup_project.sh"
SETUP_MASTER         = "setup/setup_master.sh"
SETUP_NODE_1         = "setup/setup_node_1.sh"
SETUP_NODE_2         = "setup/setup_node_2.sh"

Vagrant.configure("2") do |config|
  config.vm.box = "ubuntu/focal64"
  config.vm.synced_folder ".", "/home/vagrant/workspace", type: "virtualbox"
  
  # Provisioning de base pour tous (setup_project.sh)
  config.vm.provision "shell", path: SETUP_PROJECT_SCRIPT

  # Définition du master (control plane uniquement)
  config.vm.define "master" do |master_config|
    master_config.vm.hostname = "master"
    master_config.vm.network "private_network", ip: "192.168.56.10"
    master_config.vm.provision "shell", path: SETUP_MASTER
    master_config.vm.provider "virtualbox" do |vb|
      vb.name = "vm-master"
      vb.memory = 2048
      vb.cpus  = 2
    end
  end

  # Node-App (Web Game + PostgreSQL)
  config.vm.define "node-app" do |node_app_config|
    node_app_config.vm.hostname = "node-app"
    node_app_config.vm.network "private_network", ip: "192.168.56.11"
    node_app_config.vm.provision "shell", path: SETUP_NODE_1
    node_app_config.vm.provider "virtualbox" do |vb|
      vb.name = "vm-node-app"
      vb.memory = 1096
      vb.cpus  = 1
    end

    # Trigger uniquement après que cette VM soit UP
    node_app_config.trigger.after :up do |trigger|
      trigger.name = "Copier la clé SSH du master vers les noeuds depuis node-app"
      trigger.run = {
        inline: <<-SHELL
          echo "[Trigger - node-app] Lancement du ssh-copy-id depuis le master..."
          vagrant ssh master -c "bash /home/vagrant/workspace/setup/setup_ssh_nodes.sh"
        SHELL
      }
    end
  end

  # Node-Monitoring (Prometheus + Grafana)
  config.vm.define "node-monitoring" do |node_monitoring_config|
    node_monitoring_config.vm.hostname = "node-monitoring"
    node_monitoring_config.vm.network "private_network", ip: "192.168.56.12"
    node_monitoring_config.vm.provision "shell", path: SETUP_NODE_2
    node_monitoring_config.vm.provider "virtualbox" do |vb|
      vb.name = "vm-node-monitoring"
      vb.memory = 1096
      vb.cpus  = 1
    end

    # Trigger uniquement après que cette VM soit UP
    node_monitoring_config.trigger.after :up do |trigger|
      trigger.name = "Copier la clé SSH du master vers les noeuds depuis node-monitoring"
      trigger.run = {
        inline: <<-SHELL
          echo "[Trigger - node-monitoring] Lancement du ssh-copy-id depuis le master..."
          vagrant ssh master -c "bash /home/vagrant/workspace/setup/setup_ssh_nodes.sh"
        SHELL
      }
    end
  end
end
