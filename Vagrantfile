# -*- mode: ruby -*-
# vi: set ft=ruby :

# All Vagrant configuration is done below. The "2" in Vagrant.configure
# configures the configuration version (we support older styles for
# backwards compatibility). Please don't change it unless you know what
# you're doing.
Vagrant.configure(2) do |config|
  # The most common configuration options are documented and commented below.
  # For a complete reference, please see the online documentation at
  # https://docs.vagrantup.com.

  # Every Vagrant development environment requires a box. You can search for
  # boxes at https://atlas.hashicorp.com/search.
  config.vm.box = "attilagyorffy/FreeBSD-10.2-RELEASE-amd64"

  # Explicitly set the SSH shell to use. Given that vagrant is using `bash -l`
  # as the default (which is not installed on the default box) we override this.
  # See https://docs.vagrantup.com/v2/vagrantfile/ssh_settings.html
  config.ssh.shell = '/bin/sh'

  # Disable automatic box update checking. If you disable this, then
  # boxes will only be checked for updates when the user runs
  # `vagrant box outdated`. This is not recommended.
  # config.vm.box_check_update = false

  # Create a forwarded port mapping which allows access to a specific port
  # within the machine from a port on the host machine. In the example below,
  # accessing "localhost:8080" will access port 80 on the guest machine.
  # config.vm.network "forwarded_port", guest: 80, host: 8080

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
  config.vm.synced_folder ".", "/vagrant", disabled: true

  # Provider-specific configuration so you can fine-tune various
  # backing providers for Vagrant. These expose provider-specific options.
  # Example for VirtualBox:
  #
  # config.vm.provider "virtualbox" do |vb|
  #   # Display the VirtualBox GUI when booting the machine
  #   vb.gui = true
  #
  #   # Customize the amount of memory on the VM:
  #   vb.memory = "1024"
  # end
  #
  # View the documentation for the provider you are using for more
  # information on available options.

  # Define a Vagrant Push strategy for pushing to Atlas. Other push strategies
  # such as FTP and Heroku are also available. See the documentation at
  # https://docs.vagrantup.com/v2/push/atlas.html for more information.
  # config.push.define "atlas" do |push|
  #   push.app = "YOUR_ATLAS_USERNAME/YOUR_APPLICATION_NAME"
  # end

  # Enable provisioning with a shell script. Additional provisioners such as
  # Puppet, Chef, Ansible, Salt, and Docker are also available. Please see the
  # documentation for more information about their specific syntax and use.
  # config.vm.provision "ansible" do |ansible|
    # ansible.playbook = "ansible/site.yml"
  # end

  # Enable provisioning with a shell script.
  # See https://forums.freebsd.org/threads/how-to-setup-a-git-repository.10810/
  config.vm.provision "shell", inline: <<-SHELL
    sudo pkg upgrade -y
    sudo pkg install -y git

    # Install fish shell
    sudo pkg install -y fish

    # Create a git user and group with uid and gid as 9418
    sudo pw groupadd -n git -g 9418
    # Create a git user, set home as /git/ and set git-shell to be the shell of choice
    sudo pw useradd -n git -u 9418 -g git -c git -d /git -s /usr/local/libexec/git-core/git-shell -h -

    # Create attila user as a member of wheel
    sudo pw useradd attila -g wheel -s /usr/local/bin/fish -d /home/attila -m
    # Ensure attila is part of the git group
    sudo pw usermod attila -G git

    # Create and set permissions on the directory that will hold repos
    sudo mkdir -p /git/base
    sudo chown git:git /git/base
    sudo chmod 755 /git
    sudo chmod 775 /git/base

    # We'll be using SSH keys for authenication
    sudo mkdir /git/.ssh
    sudo chmod 700 /git/.ssh
    sudo touch /git/.ssh/authorized_keys

    # Add attila's key to the authorized_keys:
    sudo grep -q "attila@fukiBookPro.local" /git/.ssh/authorized_keys; [ $? -eq 0 ] && echo "authorized keys OK" || echo '#{File.read(File.expand_path('~/.ssh/id_rsa.pub'))}' | sudo tee -a /git/.ssh/authorized_keys

    sudo chmod 600 /git/.ssh/authorized_keys
    sudo chown -R git:git /git/.ssh

    # Create a test repo:
    sudo -H -u git mkdir -p /git/base/test.git
    cd /git/base/test.git; sudo -H -u git git init --bare --shared
  SHELL
end
