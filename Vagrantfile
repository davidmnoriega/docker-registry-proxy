registery_disk = '.vagrant/registery_disk.vdi'

Vagrant.configure(2) do |config|
  config.vm.box = "ubuntu/trusty64"
  config.vm.hostname = "registery.local"
  config.vm.provider "virtualbox" do |v|
    unless File.exist?(registery_disk)
      v.customize ['createhd', '--filename', registery_disk, '--size', 50 * 1024]
    end
    v.customize ['storageattach', :id, '--storagectl', 'SATAController', '--port', 1, '--device', 0, '--type', 'hdd', '--medium', registery_disk]
  end
  config.vm.provision "shell", inline: <<-EOF
    export disk=`lsblk -nr -o NAME|tail -1`
    parted -s /dev/$disk mklabel gpt
    parted -s /dev/$disk mkpart primary ext4 1MiB 100%
    mkfs.ext4 /dev/$disk"1"
    mkdir /data
    echo "/dev/$disk"1" /data ext4 defaults 0 0" >> /etc/fstab
    mount /data
  EOF
end
