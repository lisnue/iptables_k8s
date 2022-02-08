Vagrant.configure("2") do |config|
  config.vm.box = "ubuntu/focal64"
  config.vm.box_version = "20211004.0.0"
  config.vm.provision "shell", inline: "echo 'sudo su -' >> .bashrc"
  config.vm.provision "shell", inline: "apt update"
  config.vm.provision "shell", inline: "apt install -y conntrack bridge-utils net-tools jq resolvconf tree"
  config.vm.provision "shell", inline: "echo 'nameserver 8.8.8.8' > /etc/resolvconf/resolv.conf.d/head"
  config.vm.provision "shell", inline: "resolvconf -u"
  config.vm.provision "shell", inline: "curl -fsSL https://get.docker.com | sh"
  config.vm.synced_folder "./", "/vagrant", disabled: true

  config.vm.define "vm1" do |vm1|
    vm1.vm.hostname = "docker1"
    vm1.vm.network "private_network", ip: "192.168.50.10"
    vm1.vm.network "forwarded_port", guest: 22, host: 60610, auto_correct: true, id: "ssh"
    vm1.vm.provider :virtualbox do |spec|
      spec.cpus = 2
      spec.memory = 2048
      spec.name = "docker1"
      spec.linked_clone = true
    end
  end
  config.vm.define "vm2" do |vm2|
    vm2.vm.hostname = "docker2"
    vm2.vm.network "private_network", ip: "192.168.50.20"
    vm2.vm.network "forwarded_port", guest: 22, host: 60620, auto_correct: true, id: "ssh"
    vm2.vm.provider :virtualbox do |spec|
      spec.cpus = 2
      spec.memory = 2048
      spec.name = "docker2"
      spec.linked_clone = true
    end
  end
end
