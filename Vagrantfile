Vagrant.configure(2) do |config|
  config.vm.box = "bento/ubuntu-14.04"
  config.vm.hostname = "registry.local"

  # https://github.com/smdahlen/vagrant-hostmanager/issues/86
  config.hostmanager.enabled = true
  config.hostmanager.manage_host = true
  config.hostmanager.include_offline = true
  cached_addresses = {}
  config.hostmanager.ip_resolver = proc do |vm, resolving_vm|
    if cached_addresses[vm.name].nil?
      if hostname = (vm.ssh_info && vm.ssh_info[:host])
        vm.communicate.execute("ip -f inet -o addr show eth1") do |type, contents|
          cached_addresses[vm.name] = contents.split()[3][/[^\/]+/]
        end
      end
    end
    cached_addresses[vm.name]
  end

  config.vm.network :private_network, type: "dhcp"

  config.vm.provision "shell" do |sh|
    sh.inline = <<-EOF
    if [ ! -d "/data" ]; then
      mkdir /data
    fi
    if [ ! -f "/data/insecure_config.yml" ]; then
      cp /vagrant/insecure_config.yml /data
    fi
    EOF
  end

  config.vm.provision "docker" do |docker|
    docker.run "v2-mirror",
      image: "registry:2.2.1",
      cmd: "/var/lib/registry/insecure_config.yml",
      daemonize: true,
      args: "-p 5000:5000 -v /data:/var/lib/registry"
  end
end
