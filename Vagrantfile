# Base Image
BOX_IMAGE = "ubuntu/focal64"
BOX_VERSION = "20211026.0.0"

# max number of worker nodes
N = 2

Vagrant.configure("2") do |config|
#-----Manager Node
    config.vm.define "k8s-m" do |subconfig|
      subconfig.vm.box = BOX_IMAGE
      subconfig.vm.box_version = BOX_VERSION
      subconfig.vm.provider "virtualbox" do |v|
        v.customize ["modifyvm", :id, "--groups", "/Istio-Lab"]
        v.customize ["modifyvm", :id, "--nicpromisc2", "allow-all"]
        v.name = "Istio-k8s-m"
        v.memory = 2048
        v.cpus = 2
        v.linked_clone = true
      end
      subconfig.vm.hostname = "k8s-m"
      subconfig.vm.synced_folder "./", "/vagrant", disabled: true
      subconfig.vm.network "private_network", ip: "192.168.10.10"
      subconfig.vm.network "forwarded_port", guest: 22, host: 50010, auto_correct: true, id: "ssh"
      subconfig.vm.provision "shell", path: "https://raw.githubusercontent.com/gasida/NDKS/main/10/init_cfg.sh", args: N
      subconfig.vm.provision "shell", path: "https://raw.githubusercontent.com/gasida/NDKS/main/10/master.sh"
    end

#-----Worker Node Subnet1
(1..N).each do |i|
    config.vm.define "k8s-w#{i}" do |subconfig|
      subconfig.vm.box = BOX_IMAGE
      subconfig.vm.box_version = BOX_VERSION
      subconfig.vm.provider "virtualbox" do |v|
        v.customize ["modifyvm", :id, "--groups", "/Istio-Lab"]
        v.customize ["modifyvm", :id, "--nicpromisc2", "allow-all"]
        v.name = "Istio-k8s-w#{i}"
        v.memory = 2560
        v.cpus = 2
        v.linked_clone = true
      end
      subconfig.vm.hostname = "k8s-w#{i}"
      subconfig.vm.synced_folder "./", "/vagrant", disabled: true
      subconfig.vm.network "private_network", ip: "192.168.10.10#{i}"
      subconfig.vm.network "forwarded_port", guest: 22, host: "5001#{i}", auto_correct: true, id: "ssh"
      subconfig.vm.provision "shell", path: "https://raw.githubusercontent.com/gasida/NDKS/main/10/init_cfg.sh", args: N
      subconfig.vm.provision "shell", path: "https://raw.githubusercontent.com/gasida/NDKS/main/Calico-k8s/worker.sh"
    end

#-----Router Node
    config.vm.define "k8s-rtr" do |subconfig|
      subconfig.vm.box = BOX_IMAGE
      subconfig.vm.box_version = BOX_VERSION
      subconfig.vm.provider "virtualbox" do |v|
        v.customize ["modifyvm", :id, "--groups", "/Istio-Lab"]
        v.name = "Istio-k8s-rtr"
        v.memory = 512
        v.cpus = 1
        v.linked_clone = true
      end
      subconfig.vm.hostname = "k8s-rtr"
      subconfig.vm.synced_folder "./", "/vagrant", disabled: true
      subconfig.vm.network "private_network", ip: "192.168.10.254"
      subconfig.vm.network "forwarded_port", guest: 22, host: 50000, auto_correct: true, id: "ssh"
      subconfig.vm.provision "shell", path: "https://raw.githubusercontent.com/gasida/NDKS/main/10/linux_router.sh" 
    end

  end
end
