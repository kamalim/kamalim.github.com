---
layout: post
title: "How to create you own vagrant base boxes"
tags: "Vagrantguide-1.0"
---
{% include JB/setup %}

This blog post about creating your own vagrant base boxes from an existing Virtual Box VM .Though the official vagrant site already speaks about creating linux base boxes, here are some basic steps and reqirement that will help you creating both windows and linux (ubuntu,centos) base boxes.
This blogpost will include the steps that I used to create a ubuntu and a windows 7 base box in one of my personal projects.


### *Windows (Win 7 Enterprise 32 bit) base box creation*
---------------------------------------------------------

  1.We will be using vagrant-windows for creating the windows base box.We will require to install the below two gems:
   
    $gem install vagrant-windows
    $gem install winrm
  
  2.Install windows 7 on your virtual box VM.Make sure you enable network type as NAT.

  3.Enable WinRM with basic auth.Login as admin and run the following commands from cmd.

    winrm quickconfig -q   
    winrm set winrm/config/winrs @{MaxMemoryPerShellMB="512"}    
    winrm set winrm/config @{MaxTimeoutms="1800000"}    
    winrm set winrm/config/service @{AllowUnencrypted="true"}   
    winrm set winrm/config/service/auth @{Basic="true"}

  4.In order to share folders you must install the "Guest Additions" from within Virtual Box:

    Select the windows VM from Virtual Box ui.From the Virtual Box menu go to: 
    "Devices"->"CD/DVD Devices"->"Remove disk from virtual drive" 
    "Devices"  -> "Install Guest Additions"
  
  5.If you will be using Chef to manage the configuration then you can add this as part of the base image:
  Install the Windows Chef Client from:
  
  <http://wiki.opscode.com/display/chef/Installing+Chef+Client+on+Windows>
    

  6.Disable the below services in the windows VM:

    *UAC (type UAC in windows explorer)
    *Complex passwords.(from explorere: gpedit.msc; Computer Configuration >windows settings >security settings > Password Policy > Password Must Meet Complexity Requirement > Disabled)

  7.Create a local admin account with username/password as vagrant/vagrant (set password to not expire and uncheck must change at next login).

  8.Now the base box is ready for packaging.

    $mkdir vagrant_repo
    $vagrant init
    $vagrant package --base <name of the virtual box vm> --output windows.box 

  9.Add the new base box.

    $vagrant box add windows-7 ../vagrant_repo/windows.box
    $mkdir windows
    $cd windows
    $vagrant init ## This will create a Vagrantfile in that directory
    $vi Vagrantfile

    ## Add the below line in your Vagrantfile
    
    # -*- mode: ruby -*-
    # vi: set ft=ruby :

    Vagrant::Config.run do |config|

    config.vm.define :win1 do |win1_config|
        win1_config.vm.guest = :windows
        win1_config.vm.box = "windows-7"  
        win1_config.vm.forward_port 3389, 3390, :name => "rdp", :auto => true
        win1_config.vm.forward_port 5985, 5985, :name => "winrm", :auto => true

    $vagrant up    



So your windows vm is now ready.Lets move on to Linux Base box creation


### *Linux (Ubuntu 12.10 64 bit) base box creation*
---------------------------------------------------

  1.Create new machine in Virtual Box.

  2.Keep the networking type as nat.

  3.Install Ubuntu 12.10 on it.The ISO can be found @: 

  <http://www.ubuntu.com/download>

  4.Create a user for vagrant.For ease of distribution keep the username/password as vagrant.

  5.Enable ssh_key_base authentication for the VM by copying your ssh public key to the vagrant users home directory .This can be done as below:

    $echo > id_rsa.pub /home/vagrant/.ssh/authorized_keys
    $chmod –R 700 .ssh
    $chmod 644 .ssh/authorized_keys

  6.Edit the sudoers file as below:

    $visudo
    ##comment out the line below:

    Defaults requiretty
     
    ##Add the below lines:

    $vagrant ALL=(ALL:ALL) ALL
    $vagrant ALL=NOPASSWD: ALL


  7.Install Virtual Box Guest Additions 


  <http://www.virtualbox.org/manual/ch04.html#idp12039536>


  8.Once the above steps are complete proceed with packaging the box as below,

    $mkdir vagrant_repo
    $cd vagrant_repo
    $vagrant init ## This will create a Vagrantfile which will serve as the default vagrantfile.

    ##copy the id_rsa into the vagrant_repo directory

    $vagrant package --base <name of the virtual box vm> --output ubuntu.box --Vagrantfile Vagrantfile --include id_rsa

   9.This will create a vagrant base box ubuntu.box in the vagrant_repo directory.

   10.Add the Vagrant box to your project:

     $mkdir my_project
     $cd my_project
     $vagrant box add ubuntu ~/vagrant_repo/ubuntu.box
     $vagrant init

     ##Add the below lines in your Vagrantfile

     config.vm.define :vm1 do |vm1_config|
       vm1_config.vm.box = "ubuntu"

     $vagrant up

   Thus the ubuntu base box is created and added to a project.



   








   