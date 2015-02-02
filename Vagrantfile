Vagrant.configure("2") do |config|
  config.vm.hostname = 'go'
  config.vm.box = 'opscode-fedora-20'
  config.vm.box_url = 'http://opscode-vm-bento.s3.amazonaws.com/vagrant/virtualbox/opscode_fedora-20_chef-provisionerless.box'

  if Vagrant.has_plugin?("vagrant-cachier")
    config.cache.scope = :machine
    config.cache.auto_detect = true
  end

  # support docker
  config.vm.provider :docker do |docker, override|
    override.vm.box = 'dummy'
    override.vm.box_url = 'http://bit.ly/vagrant-docker-dummy'
    docker.image = "tlalexan/vagrant-centos:latest"
  end

  # Support testing roles by themselves.  You can't specify this on
  # command line, but can via environment variable.  This assumes
  # you've checked out into a directory matching the role name
  # (ansible-gocd)
  ENV['ANSIBLE_ROLES_PATH'] = '..'

  # configure ansible...
  config.vm.provision "ansible" do |ansible|
    ansible.host_key_checking = false
    ansible.playbook = "site.yml"
    # Set tags to either server or agent as needed.  Default is both.
    # ansible.tags = "server,agent"
    ansible.sudo = true
    ansible.verbose = ''
    # Use this if you want to override the Role defaults for example
    # to force a specific number of agents.
    ansible.extra_vars = {
      GOCD_ADMIN_EMAIL: 'david.rupp@viasat.com',
      GOCD_AGENT_INSTANCES: 4,
      GOCD_SCM_GIT: true
    }
  end

  # machines are defined here...

  config.vm.provider :virtualbox do |v|
    v.customize ["modifyvm", :id, "--memory", "2048"]
  end
  config.vm.network :private_network, ip: "192.168.50.2"
  config.vm.network "forwarded_port", guest:8153, host: 8153
end
