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
-------------------------------
    sestatus: command to view the current SELinux status
    =========
    setenforce: command to switch between permissive and enforcing mode through cli.
    ===========
    But the changes wont exist on system reboot.To make the change permanent edit the file /etc/selinux/config.

By design, SELinux allows different policies to be written that are interchangeable. The default policy in CentOS 4, 5 and 6 is the targeted policy which "targets" and confines selected system processes.

###SElinux Access Control:###

**Type Enforcement (TE):** Type Enforcement is the primary mechanism of access control used in the targeted policy

**Role-Based Access Control (RBAC):** Based around SELinux users (not necessarily the same as the Linux user), but not used in the default targeted policy

**Multi-Level Security (MLS):** Not commonly used and often hidden in the default targeted policy. 
------------------------------

In our blog we will emphasize mostly on Type Enforcement .
All system level processes and files have an SELinux security context.
Inorder to understand SELinux and its policies better , let's take a very common use case that some of us would have already encountered : 

***Httpd***

If you have a linux vm (Centos.RHEL etc) you can install httpd as below:
    $yum install httpd
    This will install httpd in your machine and created the related folder under /etc/httpd

The default document root for httpd is : */var/www/html* and the home page defaults to */var/www/html/index.html*. More details can be found in /etc/httpd/conf/httpd.conf.We can see the SELinux security context for this document root as below:

$ls -Z /var/www/html


$ls -Z /var/www/html




