---
layout: page
title: Setup local dev box using vagrant
tagline: Vagrantguide
---
{% include JB/setup %}

    
## Prerequisites in local laptop:

  1.Install Vagrant 

    http://downloads.vagrantup.com/

  2.Install Oracle Virtual Box 

    https://www.virtualbox.org/wiki/Downloads

  3.Install chef-client 

    http://wiki.opscode.com/display/chef/Installation

## Steps to setup Dev environment:

  1.Clone the chef repo:

    git clone git@your-chef-repo.git  

  2.Copy  Vagrant box from location below:

    git clone git@github.com:kamalim/Vagrant.git

  3.Adding the vagrant box:

    $cd Vagrant
    $vagrant box add centos-chef centos_with_chef.box

  4. Create vagrant project:

    $mkdir -p ~/dev
    $cd ~/dev
    $vagrant init 


##   The above command creates a Vagrantfile in “dkit-dev” directory add the below configs in the Vagrantfile.
##   This Vagrantfile config is for creating one vm.If you want to create multiple vms please refer #to the sample Vangrantfile uploaded in Devops Drive



  Vagrant::Config.run do |config|
 
    config.vm.define :dkitdev do |dkitdev_config|
       dkitdev_config.vm.box = "centos-chef"
       dkitdev_config.vm.boot_mode = :gui
       dkitdev_config.ssh.private_key_path = '/path to/ vagrant folder/ copied in step 1/id_rsa'
       dkitdev_config.vm.network :bridged, :bridge => "<respective ethernet port on laptop>"   e.g : "en3: USB Ethernet 2"
       dkitdev_config.vm.host_name = "dkit-dev"
    end
    config.vm.provision :chef_solo do |chef|
        chef.cookbooks_path = "..<path to the chef-repo clone>/cookbooks"
        chef.roles_path = "..<path to the chef-repo clone>/roles"
        chef.add_role "indexserver”
        chef.add_role "access_controller"
        chef.add_role "readerprod"
    end


##  Once the above configs are added, save and close the vagrantfile and run vagrant up.This ##will create a vm with the respective role mentioned in the config.


    $vagrant up

  5.Once the vagrant vm is ready, the application can be deployed in the VM.


