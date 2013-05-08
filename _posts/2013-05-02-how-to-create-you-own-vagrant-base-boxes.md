---
layout: post
title: "How to create you own vagrant base boxes"
tags: "Vagrantguide"
---
{% include JB/setup %}

This blog post about creating your own vagrant base boxes from an existing Virtual Box VM .Though the official vagrant site already speaks about creating linux base boxes, here are some basic steps and reqirement that will help you creating both windows and linux (ubuntu,centos) base boxes.
This blogpost will include the steps that I used to create a ubuntu and a windows 7 base box in one of my personal projects.


#### Linux (Ubuntu 12.10 64 bit) base box creation:

  1.Create new machine in Virtual Box.

  2.Keep the networking type as nat.

  3.Install Ubuntu 12.10 on it.The ISO can be found here http://www.ubuntu.com/download. 

  4.Create a user for vagrant.For ease of distribution keep the username/password as vagrant.

  5.Enable ssh_key_base authentication for the VM by copying your ssh public key to the vagrant users home directory .This can be done as below:

      $echo > id_rsa.pub /home/vagrant/.ssh/authorized_keys
	  $chmod –R 700 .ssh
	  $chmod 644 .ssh/authorized_keys

  6.Edit the sudoers file as below:

      $visudo
      ##comment out the line below:
      Defaults requiretty
     
      ## Add the below lines:
      $vagrant ALL=(ALL:ALL) ALL
      $vagrant ALL=NOPASSWD: ALL


  7.Install Virtual Box Guest Additions <a href="http://www.virtualbox.org/manual/ch04.html#idp12039536">http://www.virtualbox.org/manual/ch04.html#idp12039536</a>






   