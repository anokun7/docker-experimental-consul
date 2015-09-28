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
