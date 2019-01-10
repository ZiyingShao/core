# -*- mode: ruby -*-
# vi: set ft=ruby :

# Require the AWS provider plugin
require 'vagrant-aws'

# All Vagrant configuration is done below. The "2" in Vagrant.configure
# configures the configuration version (we support older styles for
# backwards compatibility). Please don't change it unless you know what
# you're doing.
Vagrant.configure("2") do |config|
  # The most common configuration options are documented and commented below.
  # For a complete reference, please see the online documentation at
  # https://docs.vagrantup.com.

  # Every Vagrant development environment requires a box. You can search for
  # boxes at https://vagrantcloud.com/search.
  config.vm.box = "aws-example"

  config.vm.synced_folder ".", "/vagrant", disabled: false, type: 'rsync'
  # Disable automatic box update checking. If you disable this, then
  # boxes will only be checked for updates when the user runs
  # `vagrant box outdated`. This is not recommended.
  # config.vm.box_check_update = false

  # Create a forwarded port mapping which allows access to a specific port
  # within the machine from a port on the host machine. In the example below,
  # accessing "localhost:8080" will access port 80 on the guest machine.
  # NOTE: This will enable public access to the opened port
  # config.vm.network "forwarded_port", guest: 80, host: 8080

  # Create a forwarded port mapping which allows access to a specific port
  # within the machine from a port on the host machine and only allow access
  # via 127.0.0.1 to disable public access
  # config.vm.network "forwarded_port", guest: 80, host: 8080, host_ip: "127.0.0.1"

  # Create a private network, which allows host-only access to the machine
  # using a specific IP.
  # config.vm.network "private_network", ip: "192.168.33.10"

  # Create a public network, which generally matched to bridged network.
  # Bridged networks make the machine appear as another physical device on
  # your network.
  # config.vm.network "public_network"

  # Share an additional folder to the guest VM. The first argument is
  # the path on the host to the actual folder. The second argument is
  # the path on the guest to mount the folder. And the optional third
  # argument is a set of non-required options.
  # config.vm.synced_folder "../data", "/vagrant_data"

  # Provider-specific configuration so you can fine-tune various
  # backing providers for Vagrant. These expose provider-specific options.
  # Example for VirtualBox:
  #
  config.vm.provider "aws" do |aws,override|
    # Read AWS authentication information from environment variables
    aws.access_key_id = ENV['AWS_MICHAEL_ACCESS_KEY_ID']
    aws.secret_access_key = ENV['AWS_MICHAEL_SECRET_ACCESS_KEY']

    # Specify SSH keypair to use
    aws.keypair_name = 'michael-key-pair-london'

    # Specify region, AMI ID, Instance, and security groups
    aws.region = 'eu-west-2'
    aws.ami = 'ami-0274e11dced17bb5b'
    aws.instance_type = 't2.micro'
    aws.security_groups = ['michael-test-london']

    # Specify username and private key path
    override.ssh.username = 'ec2-user'
    override.ssh.private_key_path = '/Users/Michael/Documents/aws/michael-key-pair-london.pem'
  end 
  # Enable provisioning with a shell script. Additional provisioners such as
  # Puppet, Chef, Ansible, Salt, and Docker are also available. Please see the
  # documentation for more information about their specific syntax and use.
  # config.vm.provision "shell", inline: <<-SHELL
  #   apt-get update
  #   apt-get install -y apache2
  # SHELL
  config.vm.provision "shell", inline: <<-SHELL #, :privileged => false
    sudo yum -y update
    sudo yum -y upgrade
    sudo yum -y install tmux
    sudo yum -y groupinstall "Development Tools"  # similar to "build-essential"
    sudo yum -y install libmpc-devel mpfr-devel gmp-devel
    sudo yum -y install ncurses-devel # Debian "libncurses-dev"
    sudo yum -y install openssl-devel # Debian "libssl-dev"
    sudo yum -y install zlib-devel  # Debian "libz-dev"
    sudo yum -y install cmake gcc wget git bc nano patch
    sudo yum -y install python-pip python-devel
    pip install --upgrade pip
    pip install numpy scipy scikit-learn
    # not installed: clang g++-4.9 mosquitto sparse flawfinder

    # start the toy experiment
    git clone https://github.com/crimson-unicorn/toy.git data
    mkdir build
    cd build
    git clone https://github.com/crimson-unicorn/graphchi-cpp
    git clone https://github.com/crimson-unicorn/modeling
    START=`date +%s`
    cd graphchi-cpp/ && make sdebug
    make run_toy
    cd ..
    cd modeling && python model.py --train_dir ../../data/train_toy/ --test_dir ../../data/test_toy/
    END=`date +%s`
    echo $((END-START))
    # clean up
    cd ../../data/
    rm -rf test_toy/
    rm -rf train_toy/
  SHELL
end
