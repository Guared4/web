Vagrant.configure("2") do |config|
  config.vm.define "DynamicWeb" do |vmconfig|
    vmconfig.vm.box = "bento/ubuntu-22.04"
    vmconfig.vm.hostname = "DynamicWeb"
    vmconfig.vm.network "private_network", ip: "192.168.57.10"

    vmconfig.vm.network "forwarded_port", guest: 8083, host: 8083
    vmconfig.vm.network "forwarded_port", guest: 8081, host: 8081
    vmconfig.vm.network "forwarded_port", guest: 8082, host: 8082

    vmconfig.vm.provider "virtualbox" do |vbx|
      vbx.memory = "2048"
      vbx.cpus = "2"
      vbx.customize ["modifyvm", :id, "--audio", "none"]
    end

    ssh_pub_key = File.readlines("./id_rsa/id_rsa.pub").first.strip
    vmconfig.vm.provision "shell", inline: <<-SHELL
      echo #{ssh_pub_key} >> ~vagrant/.ssh/authorized_keys
      echo #{ssh_pub_key} >> ~root/.ssh/authorized_keys
      sudo sed -i 's/\#PasswordAuthentication no/PasswordAuthentication yes/g' /etc/ssh/sshd_config
      sudo systemctl restart sshd
    SHELL
  end
end