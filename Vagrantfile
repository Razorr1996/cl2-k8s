# -*- mode: ruby -*-
# vi: set ft=ruby :

PREFIX = "cl2"

CPU_CORES = 2
MEM_IN_MB = 4096

CONTROL_NUM = 1
WORKER_NUM = 0

Vagrant.configure("2") do |config|
  # The most common configuration options are documented and commented below.
  # For a complete reference, please see the online documentation at
  # https://docs.vagrantup.com.

  # Every Vagrant development environment requires a box. You can search for
  # boxes at https://vagrantcloud.com/search.
  config.vm.box = "bento/ubuntu-22.04-arm64"

  # Disable automatic box update checking. If you disable this, then
  # boxes will only be checked for updates when the user runs
  # `vagrant box outdated`. This is not recommended.
  # config.vm.box_check_update = false

  config.dns.tld = PREFIX

  # Provider-specific configuration so you can fine-tune various
  # backing providers for Vagrant. These expose provider-specific options.
  config.vm.provider "parallels" do |prl|
    prl.cpus = CPU_CORES
    prl.memory = MEM_IN_MB

    # Uncomment for updating Parallels Tools
    # prl.update_guest_tools = true

    prl.customize "post-import", ["set", :id, "--nested-virt", "on"]
  end

  # Enable provisioning with a shell script. Additional provisioners such as
  # Ansible, Chef, Docker, Puppet and Salt are also available. Please see the
  # documentation for more information about their specific syntax and use.
  # config.vm.provision "shell", inline: <<-SHELL
  #   apt-get update
  #   apt-get install -y apache2
  # SHELL

  config.vm.provision "ansible" do |ansible|
    ansible.playbook = "playbook.yml"
    ansible.compatibility_mode = "2.0"
  end

  control_nodes = (1..CONTROL_NUM).map { |i| "#{PREFIX}-control#{i}" }
  worker_nodes = (1..WORKER_NUM).map { |i| "#{PREFIX}-worker#{i}" }

  (control_nodes + worker_nodes).each { |node_name|
    config.vm.define node_name do |cfg|
      cfg.vm.hostname = node_name

      cfg.vm.provider "parallels" do |prl|
        prl.name = node_name
      end
    end
  }
end
