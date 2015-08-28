# -*- mode: ruby -*-
# vi: set ft=ruby :

# Non-routable IPs used by the private network connecting the VMs.
INTERNAL_IP_NET = "10.60.0"

# 22 and 23 are supported.
FEDORA_VERSION = 22

Vagrant.configure(2) do |config|
  config.vm.box = "fedora-atomic-cloud-#{FEDORA_VERSION}"
  config.vm.box_check_update = true

  # Create a forwarded port mapping which allows access to a specific port
  # within the machine from a port on the host machine. In the example below,
  # accessing "localhost:8080" will access port 80 on the guest machine.
  # config.vm.network "forwarded_port", guest: 80, host: 8080

  config.vm.network :private_network, type: :dhcp,
      ip: "#{INTERNAL_IP_NET}.1", netmask: '255.255.255.0',
      nic_type: 'virtio'

  # Create a public network, which generally matched to bridged network.
  # Bridged networks make the machine appear as another physical device on
  # your network.
  # config.vm.network "public_network"

  config.vm.synced_folder '.', '/vagrant', disabled: true
  config.vm.synced_folder '.', '/home/vagrant/sync', disabled: true

  config.vm.define "master" do |master|
  end

  config.vm.define "worker1" do |worker|
    # NOTE: The ansible provisioning block is only defined in the last worker,
    #       so it runs after all the VMs have been created.
    config.vm.provision :ansible do |ansible|
      ansible.playbook = "k8s.yml"
      ansible.limit = "all"

      # Simulate the groups produced by the OpenStack setup.
      ansible.groups = {
        "meta-system_role_alg_master" => ["master"],
        "meta-system_role_alg_worker" => ["worker1"],
      }
      ansible.extra_vars = {
        os_image_user: "vagrant",
        internal_net_cidr: "#{INTERNAL_IP_NET}.0/24",
      }

      # Uncomment for Ansible connection debugging.
      # ansible.verbose = "vvvv"
    end
  end

  config.vm.provider :virtualbox do |vb, override|
    vb.memory = 1024
    case FEDORA_VERSION
    when 22
      override.vm.box_url = "https://download.fedoraproject.org/pub/fedora/linux/releases/22/Cloud/x86_64/Images/Fedora-Cloud-Atomic-Vagrant-22-20150521.x86_64.vagrant-virtualbox.box"
    when 23
      override.vm.box_url = "https://download.fedoraproject.org/pub/fedora/linux/releases/test/23_Alpha/Cloud/x86_64/Images/Fedora-Cloud-Atomic-Vagrant-23_Alpha-20150805.x86_64.vagrant-virtualbox.box"
    end

    # The NAT network adapter uses the Intel PRO/1000MT adapter by default.
    vb.customize ['modifyvm', :id, '--nictype1', 'virtio']
  end

  config.vm.provider :libvirt do |domain, override|
    domain.memory = 1024
    case FEDORA_VERSION
    when 22
      override.vm.box_url = "https://download.fedoraproject.org/pub/fedora/linux/releases/22/Cloud/x86_64/Images/Fedora-Cloud-Atomic-Vagrant-22-20150521.x86_64.vagrant-libvirt.box"
    when 23
      override.vm.box_url = "https://download.fedoraproject.org/pub/fedora/linux/releases/test/23_Alpha/Cloud/x86_64/Images/Fedora-Cloud-Atomic-Vagrant-23_Alpha-20150805.x86_64.vagrant-libvirt.box"
    end
  end
end
