---
layout: page
title: How to setup dev box using Vagrant!
tagline: Vagrantguide
---
{% include JB/setup %}

    
#Prerequisites in local laptop:

  Install Vagrant : (http://downloads.vagrantup.com/)
  Install Oracle Virtual Box : (https://www.virtualbox.org/wiki/Downloads)
  Install chef-client : (http://wiki.opscode.com/display/chef/Installation)

#Steps to setup Dev environment:

  1.Clone the chef repo:

    git clone git@chef-repo.git  

  2.Copy  Vagrant box from location below:

    git@github.com:kamalim/Vagrant.git

  3.Adding the vagrant box:

    $cd Vagrant
    $vagrant box add centos-chef centos_with_chef.box

  4. Create vagrant project:

    $mkdir -p ~/dev
    $cd ~/dev
    $vagrant init 


