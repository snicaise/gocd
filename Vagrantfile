Vagrant.configure("2") do |config|
  config.vm.provider "virtualbox"
  config.vm.box = "ubuntu/trusty64"

  config.ssh.insert_key = false

  config.vm.define :goserver do |goserver|
    goserver.vm.hostname = "goserver"
    goserver.vm.network :private_network, ip: "192.168.10.2"
  end

  config.vm.define :goagent1 do |goagent1|
    goagent1.vm.hostname = "goagent"
    goagent1.vm.network :private_network, ip: "192.168.10.3"

    ENV['ANSIBLE_ROLES_PATH'] = '..'
    goagent1.vm.provision :ansible do |ansible|
      ansible.playbook = "site.yml"
      ansible.limit = 'all'
      ansible.sudo = true
      ansible.groups = {
        "goserver"            => ["goserver"],
        "goagents"            => ["goagent1"],
        "all_groups:children" => ["goserver", "goagents"]
      }
    end
  end

  config.vm.provider :virtualbox do |vb|
    vb.customize ['modifyvm', :id, '--cpus', 1]
    vb.customize ['modifyvm', :id, '--memory', 1024]
  end

  if Vagrant.has_plugin?("vagrant-hostmanager")
    config.hostmanager.enabled = true
    config.hostmanager.manage_host = true
    config.hostmanager.ignore_private_ip = false
    config.hostmanager.include_offline = true
  end

  if Vagrant.has_plugin?("vagrant-cachier")
    config.cache.scope = :box
    config.cache.auto_detect = true
  end

end
