registry_disk = '.vagrant/registry_disk.vdi'

Vagrant.configure(2) do |config|
  config.vm.box = "bento/ubuntu-14.04"
  config.vm.hostname = "registry.local"

  config.vm.provider "virtualbox" do |v|
    unless File.exist?(registry_disk)
      v.customize ['createhd', '--filename', registry_disk, '--size', 50 * 1024]
    end
    v.customize ['storageattach', :id, '--storagectl', 'SATA Controller', '--port', 1, '--device', 0, '--type', 'hdd', '--medium', registry_disk]
  end

  config.vm.provision "shell" do |sh|
    sh.inline = <<-EOF
      export disk=`lsblk -nr -o NAME|tail -1`
      parted -s /dev/$disk mklabel gpt
      parted -s /dev/$disk mkpart primary ext4 1MiB 100%
      mkfs.ext4 /dev/$disk"1"
      mkdir /data
      echo "/dev/$disk"1" /data ext4 defaults 0 0" >> /etc/fstab
      mount /data
    EOF
  end

  config.vm.provision "docker" do |docker|
    docker.pull_images  "registry:2.2.1"
  end
end
