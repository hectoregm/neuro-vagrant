# -*- mode: ruby -*-
# vi: set ft=ruby :

# Returns true if `GUI` environment variable is set to a non-empty value.
# Defaults to false
def gui_enabled?
  !ENV.fetch('GUI', '').empty?
end

$script = <<SCRIPT

# Generate locale
sudo locale-gen UTF-8

# Enable NeuroDebian repos
wget -O- http://neuro.debian.net/lists/trusty.au.full | sudo tee /etc/apt/sources.list.d/neurodebian.sources.list
sudo apt-key adv --recv-keys --keyserver hkp://pgp.mit.edu:80 2649A5A9
sudo apt-get update

# Install xfce4
sudo apt-get install -y xfce4 virtualbox-guest-dkms virtualbox-guest-utils virtualbox-guest-x11

# Enable non-console users to start X
sudo sed -i 's/console/anybody/' /etc/X11/Xwrapper.config
sudo touch /home/vagrant/.matplotlib
sudo touch /root/.matplotlib

# Install CPAC
wget https://github.com/FCP-INDI/C-PAC/blob/master/scripts/cpac_install_ubuntu.tar.gz?raw=true
tar -xzvf cpac_install_ubuntu.tar.gz?raw=true
sudo bash ./cpac_install.sh

# Fix locale
sudo locale-gen UTF-8
SCRIPT

VAGRANTFILE_API_VERSION = "2"

Vagrant.configure(VAGRANTFILE_API_VERSION) do |config|
  config.vm.box = "ubuntu/trusty64"
  
  config.vm.synced_folder "data", "/data"
  
  config.vm.provider "virtualbox" do |v|
      v.gui = gui_enabled?
  end
  
  config.vm.provision "shell", inline: $script
end
