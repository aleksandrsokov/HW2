# -*- mode: ruby -*-
# vi: set ft=ruby :

Vagrant.configure("2") do |config|
  config.vm.box = "ubuntu/focal64"
  config.vm.box_check_update = false
  config.vm.hostname = "vanilla.local"
 
  config.vm.provider "virtualbox" do |vb|
    vb.memory = "6144"
    vb.cpus = "6"
   end


  config.vm.synced_folder "C:\\VM\\vanilla\\share", "/shareData", owner: "vagrant", group: "vagrant"

  config.vm.provision "shell", inline: <<-SHELL
     sed -i 's/PasswordAuthentication no/PasswordAuthentication yes/g' /etc/ssh/sshd_config    
     systemctl restart sshd.service
    SHELL

   config.vm.provision "shell", inline: <<-SHELL
     apt update
     apt install -y wget curl build-essential libncurses-dev bison flex libssl-dev libelf-dev fakeroot
     apt install -y dwarves
     wget https://cdn.kernel.org/pub/linux/kernel/v6.x/linux-6.6.30.tar.xz
     tar -xf linux-6.6.30.tar.xz
     cd linux-6.6.30
     cp -v /boot/config-$(uname -r) .config
     make localmodconfig
     scripts/config --disable SYSTEM_TRUSTED_KEYS
     scripts/config --disable SYSTEM_REVOCATION_KEYS
     scripts/config --set-str CONFIG_SYSTEM_TRUSTED_KEYS ""
     scripts/config --set-str CONFIG_SYSTEM_REVOCATION_KEYS ""
     fakeroot make -j6
     make modules_install
     make install
     reboot
   SHELL
end
