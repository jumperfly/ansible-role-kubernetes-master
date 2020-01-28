# -*- mode: ruby -*-
# vi: set ft=ruby :

Vagrant.configure("2") do |config|
  config.vm.box = "jumperfly/etcd-base-3.3"
  config.vm.box_version = "15.4"
  config.vm.box_check_update = false
  config.ssh.insert_key = false
  config.vm.provider "virtualbox" do |v|
    v.cpus = 1
    v.memory = 2048
  end

  config.vm.hostname = "master1"

  config.vm.provision "ansible" do |ansible|
    ansible.compatibility_mode = "2.0"
    ansible.limit = "all"
    ansible.galaxy_role_file = "tests/requirements.yml"
    ansible.galaxy_command = "ansible-galaxy install --role-file=%{role_file} --roles-path=%{roles_path}"
    ansible.playbook = "tests/test.yml"
    ansible.groups = {
      "etcd_nodes" => ["default"]
    }
  end
end
