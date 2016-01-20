Vagrant.configure(2) do |config|
  config.vm.box = "bento/ubuntu-14.04"
  config.vm.hostname = "registry.local"

  config.vm.provision "shell" do |sh|
    sh.inline = <<-EOF
      mkdir /data
    EOF
  end

  config.vm.provision "docker" do |docker|
    docker.run "v2-mirror",
      image: "registry:2.2.1",
      daemonize: true,
      args: "-p 5000:5000 -v /data:/var/lib/registry -v /vagrant/insecure_config.yml:/etc/docker/registry/config.yml"
  end
end
