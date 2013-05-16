---
layout: post
title: "Getting Started with SELinux"
tags: [Security]
---
{% include JB/setup %}

We all have our own fears and concern about how SELinux works.So most of us tend to disable SELinux before we start up using a linux machine.But I think this is not the ideal solution especially with the present day situations where in we can not just leave our webservers open on to internet without any security policies applied to it.
Recently I encountered couple of scenarios where in some of the native applications(httpd,varnish) would fail if they have to midify/create any directory/files within the root directory work if the default SElinux is in enforcing mode.So instead of disabling SELinux, I kept it in permissive mode and tweaked aroud little bit with the SELinux policies.
This post is all about how we can get started with SELinux in permissive mode and slowly use as an essential security configuration in our systems.

###SeLinux has three modes:###
------------------------------
**1.Enforcing:** The default mode which will enable and enforce the SELinux security policy on the system, denying access and logging actions.

**2.Permissive:** SELinux is enabled but will not enforce the security policy, only warn and log actions.

**3.Disabled:** SELinux turned off.

####*Some useful commands:*####
    sestatus: command to view the current SELinux status
    =========
    setenforce: command to switch between permissive and enforcing mode through cli.
    ===========
    But the changes wont exist on system reboot.To make the change permanent edit the file /etc/selinux/config.

By design, SELinux allows different policies to be written that are interchangeable. The default policy in CentOS 4, 5 and 6 is the targeted policy which "targets" and confines selected system processes.

###SElinux Access Control:###

*Some of these details have been reffered from <http://wiki.centos.org/HowTos/SELinux>

**Type Enforcement (TE):** Type Enforcement is the primary mechanism of access control used in the targeted policy

**Role-Based Access Control (RBAC):** Based around SELinux users (not necessarily the same as the Linux user), but not used in the default targeted policy

**Multi-Level Security (MLS):** Not commonly used and often hidden in the default targeted policy. 

------------------------------------------------------------------------------------------------------------------

In our blog we will emphasize mostly on Type Enforcement .
All system level processes and files have an SELinux security context.
Inorder to understand SELinux and its policies better, let's go through my own experience while setting up a simple http server .


###Debugging and Troubleshooting SELinux:###
-----------------------------------------------
**Goal:** My intent was to setup a simple webserver using apache httpd, where my website contents will be there in my home directory /home/vagrant/content. With this goal in mind I started as below.

* In a linux vm (Centos.RHEL etc) install httpd as below.

    $yum install httpd

    This will install httpd in your machine and created the related  conf files under /etc/httpd
    
    $service httpd start

* Create a simple home page at /home/vagrant/index.html.

+ Change the DocumentRoot path to "/home/vagrant".The can be done by commenting out all lines in
 /etc/httpd/conf.d/welcome.conf and change the below line in /etc/httpd/conf/httpd.conf.

    DocumentRoot "/home/vagrant" 

 *The default document root for httpd is : /var/www/html* 
 *The default home page is at :/var/www/html/index.html*. 

* Now restart httpd service.

    $service httpd restart.


On trying to access the page from browser http://my.vm.ip, it gave "403 Forbidden" error as below:

---------------------------------

![screenshot3](/images/scs-3.png)

---------------------------------

I looked at logs in  /var/log/http/error_log and found  the below error:

![screenshot4](/images/scs-2.png)

This is because httpd is one of the system processed which are under targetted SELinux policy.In order to get more details , let us see the SELinux context for the default DocumentRoot for httpd (/var/www/html)

![screenshot1](/images/scs-1.png)

Whereas the SELinux context of the home directory (/home/vagrant) is 





