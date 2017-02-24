
# Setup Manaagement System Virtual Machines
#
# Initialize MaaS instance with default 2048M RAM and an image bases on Xenial
# 16.04 LTS

BOX_NAME = "geerlingguy/ubuntu1604"

GUI_MODE = false

MAAS_MEM = "2048"
MAAS_CPU = "1"
MAAS_PUBLIC_IP = "192.168.1.2"
MAAS_INTERNAL_IP = "10.0.0.2"
MAAS_DOMAIN = "cm.local"

Vagrant.configure("2") do |cfg|
  # MaaS region and rack controller
  cfg.vm.define "maas", primary: true do |maas|
    maas.vm.box = "#{BOX_NAME}"
    maas.vm.hostname = "mass.#{MAAS_DOMAIN}"
    maas.vm.network :public_network, adapter: 2, ip: "#{MAAS_PUBLIC_IP}",
      nic_type: "virtio"
    maas.vm.network :public_network, adapter: 3 , ip: "#{MAAS_INTERNAL_IP}",
      nic_type: "virtio"
    maas.vm.provider "virtualbox" do |vb|
      vb.gui = GUI_MODE
      vb.name = "mass.#{MAAS_DOMAIN}"
      vb.memory = MAAS_MEM
      vb.cpus = MAAS_CPU
      vb.customize ["modifyvm", :id, "--nicpromisc2", "allow-all"]
      vb.customize ["modifyvm", :id, "--nicpromisc3", "allow-all"]

    end

    maas.vm.synced_folder ".", "/vagrant", disabled: true

    maas.vm.provision "ansible" do |ansible|
      ansible.verbose = true
      ansible.playbook = "playbooks/maas.yml"
      ansible.raw_ssh_args = [
        "-o", "ControlMaster=auto", "-o", "ControlPersist=60s", "-o",
        "UserKnownHostsFile=/dev/null", "-o", "StrictHostKeyChecking=no"
      ]

    end
  end

  # Juju controller
  # Kubernetes master
  #
end
