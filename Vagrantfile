Vagrant.configure(2) do |config|

  (4..7).each do |i|
    config.vm.define "ubuntu-exp-#{i}" do |ubuntu|
      ubuntu.vm.box = "ubuntu/trusty64"
      ubuntu.vm.hostname = "ubuntu#{i}.exp.docker.demo"
      ubuntu.vm.network "private_network", ip: "10.10.0.#{i}"
      ubuntu.vm.network "forwarded_port", guest: 22, host: "220#{i}", auto_correct: false, id: "ssh"
  
     # Enable provisioning with a shell script 
      ubuntu.vm.provision "shell", inline: <<-SHELL
        echo root:docker | sudo chpasswd
        sudo curl -sSL https://experimental.docker.com/ | sh
        sudo cp /vagrant/docker-ubuntu /etc/default/docker
        sudo service docker restart
        sudo usermod -a -G docker vagrant

      # Install docker-compose
        sudo curl -L https://github.com/docker/compose/releases/download/1.4.2/docker-compose-`uname -s`-`uname -m` -o /usr/local/bin/docker-compose
        sudo chmod +x /usr/local/bin/docker-compose
  
      # Install consul
        sudo curl -L https://dl.bintray.com/mitchellh/consul/0.5.2_linux_amd64.zip -o /usr/local/bin/consul-0.5.2.zip
	sudo apt-get install unzip -y
	cd /usr/local/bin
	sudo unzip consul-0.5.2.zip
        sudo useradd consul
	echo consul:consul | sudo chpasswd
	mkdir /var/consul
	chown consul:consul /var/consul
	mkdir -p /etc/consul.d/{bootstrap,server,client}
  
     # Install some useful tools
        sudo apt-get install telnet strace nc lynx -y &

     # Pull some commonly used images
     #  docker pull busybox &
     #  docker pull phusion/baseimage &
     #  docker pull nginx &
      SHELL
    end
  end
end
