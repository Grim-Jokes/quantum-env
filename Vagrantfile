# -*- mode: ruby -*-
# vi: set ft=ruby :

# All Vagrant configuration is done below. The "2" in Vagrant.configure
# configures the configuration version (we support older styles for
# backwards compatibility). Please don't change it unless you know what
# you're doing.
Vagrant.configure("2") do |config|

  config.vm.box = "GrimJokes/ubuntu-16.04-gnome"

  # Create a private network, which allows host-only access to the machine
  # using a specific IP.
  config.vm.network "private_network", ip: "192.168.33.10"

  

  config.vm.provider "virtualbox" do |vb|
    # Display the VirtualBox GUI when booting the machine
    vb.gui = true
    vb.customize ["modifyvm", :id, "--cpus", "4"]   
    vb.customize ["modifyvm", :id, "--vram", "126"]
  
    # Customize the amount of memory on the VM:
    vb.memory = "4086"
  end

  config.vm.provision "file", source: "~/.ssh/github/id_rsa", destination: "~/.ssh/github/id_rsa"
  config.vm.provision "file", source: "~/.ssh/github/id_rsa.pub", destination: "~/.ssh/github/id_rsa.pub"
  config.vm.provision "file", source: "~/.ssh/config", destination: "~/.ssh/config"

  config.vm.provision "shell", inline: <<-SHELL
    apt-get -y install qt5-default libqt5webkit5-dev build-essential python-lxml python-pip xvfb
  SHELL

  config.vm.provision "file", source: "./webkit_setup.py", destination: "~/webkit-server/setup.py"
  
  config.vm.provision "shell", privileged: false, inline: <<-SHELL    
    mkdir -p ~/.ssh/github
    
    ssh-keyscan -t rsa -H github.com > ~/.ssh/known_hosts
    sudo chmod 600 ~/.ssh/github/id_rsa*
    
    sudo chown vagrant ~/.ssh/* -R
    ssh-keyscan -t rsa github.com > ~/.ssh/known_host
    rm -rf ~/quantum-api
    git clone git@github.com:Grim-Jokes/quantum-api.git
  SHELL

  config.vm.provision "shell", inline: <<-SHELL
    apt-get install postgresql-client pgadmin3 -y
  SHELL

  config.vm.provision "shell", inline: <<-SHELL
    

    add-apt-repository ppa:jonathonf/python-3.6 -y
    apt-get update -y
    apt-get install python3.6 python3-dev python3.6-dev libxml2-dev libxslt1-dev zlib1g-dev -y
    apt-get install docker docker-compose -y
    
    add-apt-repository -y "deb https://packages.microsoft.com/repos/vscode stable main"
    apt-key adv --keyserver keyserver.ubuntu.com --recv-keys EB3E94ADBE1229CF
    apt update -y
    apt -y install code
    apt-get install virtualenv -y
  SHELL

  config.vm.provision :shell, inline: <<-SCRIPT
      usermod -aG docker $(whoami)
      service docker restart
  SCRIPT

  config.vm.provision :reload

  config.vm.provision "shell", privileged: false, inline: <<-SHELL
    virtualenv -p python3.6 quantum  
    echo 'source ~/quantum/bin/activate' > activate.sh
    sudo chmod 700 activate.sh
    . activate.sh
    pip install -r ~/quantum-api/requirements/requirements.txt
    alias quit=gnome-session-quit    
  SHELL

  config.vm.provision "docker" do |d|
    d.pull_images "ubuntu:16.04"
    d.pull_images "postgres"
  end
  

  
end
