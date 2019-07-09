# -*- mode: ruby -*-
# vi: set ft=ruby :

ENV['VAGRANT_DEFAULT_PROVIDER'] = 'libvirt'
Vagrant.configure("2") do |config|
  
  num_workers = 2 
  workers = []

  (1..num_workers).each do |i|
    workers.push("worker#{i}")
  end

  groups = {
    "driver" => ["driver"] , 
    "workers" => workers, 
  }
  
  config.vm.provider :libvirt do |libvirt| 
    libvirt.uri = "qemu:///system"
    libvirt.storage_pool_name = "vagrant"
  end 

  config.vm.define :driver do |driver| 
    driver.vm.box = "centos/7"
    driver.vm.hostname = "driver"
    driver.vm.provision :ansible do |ansible|
      ansible.groups = groups
      ansible.playbook = "provisioning/playbook-driver.yml"
    end
  end 


  (1..num_workers).each do |i| 
    config.vm.define "worker#{i}" do |worker| 
      worker.vm.box = "centos/7"
      worker.vm.hostname = "worker#{i}"
    end
  end

  config.vm.provision :ansible do |ansible|
    ansible.groups = groups
    ansible.playbook = "provisioning/playbook-workers.yml"
  end

end