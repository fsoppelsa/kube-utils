# Structure for a 4-nodes static kube without HA
#
VAGRANTFILE_API_VERSION = "2"
DEFAULT_BOX = "generic/fedora25"
DOMAIN_NAME = ".infra.ci.local"
HOSTS_MEMORY = 1024
HOSTS_CPUS = 1

#############################################################################

Vagrant.configure(VAGRANTFILE_API_VERSION) do |config|
  config.ssh.shell = "bash -c 'BASH_ENV=/etc/profile exec bash'"
  config.ssh.forward_agent = true

  config.vm.provider "virtualbox" do |v|
    v.memory = HOSTS_MEMORY
    v.cpus = HOSTS_CPUS
  end

  config.vm.define "etcd" do |etcd|
    etcd.vm.box = DEFAULT_BOX
    etcd.vm.hostname = "etcd" + DOMAIN_NAME
    etcd.vm.network "private_network", ip: "192.168.11.99", netmask: "255.255.255.0"

    ## Fedora Workaround ;)
    config.vm.provision "shell", inline: <<-SHELL
      ip a add 192.168.11.99/24 dev eth1
      ip l set eth1 up
    SHELL

    config.vm.provision "ansible" do |ansible|
      ansible.playbook = "playbooks/etcd_init.yml"
    end
  end

  config.vm.define "kmaster1" do |kmaster1|
    kmaster1.vm.box = DEFAULT_BOX
    kmaster1.vm.hostname = "kmaster1" + DOMAIN_NAME
    kmaster1.vm.network "private_network", ip: "192.168.11.100", netmask: "255.255.255.0"

    ## Workaround
    config.vm.provision "shell", inline: <<-SHELL
      ip a add 192.168.11.100/24 dev eth1
      ip l set eth1 up
    SHELL

    config.vm.provision "ansible" do |ansible|
      ansible.playbook = "playbooks/k8s_init.yml"
    end
  end

  config.vm.define "knode1" do |knode1|
    knode1.vm.box = DEFAULT_BOX
    knode1.vm.hostname = "knode1" + DOMAIN_NAME
    knode1.vm.network "private_network", ip: "192.168.11.110", netmask: "255.255.255.0"

    ## Workaround
    config.vm.provision "shell", inline: <<-SHELL
      ip a add 192.168.11.110/24 dev eth1
      ip l set eth1 up
    SHELL

    config.vm.provision "ansible" do |ansible|
      ansible.playbook = "playbooks/k8s_node.yml"
    end
  end

  config.vm.define "knode2" do |knode2|
    knode2.vm.box = DEFAULT_BOX
    knode2.vm.hostname = "knode2" + DOMAIN_NAME
    knode2.vm.network "private_network", ip: "192.168.11.111", netmask: "255.255.255.0"

    ## Workaround
    config.vm.provision "shell", inline: <<-SHELL
      ip a add 192.168.11.111/24 dev eth1
      ip l set eth1 up
    SHELL

    config.vm.provision "ansible" do |ansible|
      ansible.playbook = "playbooks/k8s_node.yml"
    end
  end
end
