---
layout: post
title: "Getting Started with SELinux"
tags: Security
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
**Goal:** My intent was to setup a simple webserver using apache httpd, where my website contents will be there in my home directory /home/vagrant/html. With this goal in mind I started as below.

* In a linux vm (Centos.RHEL etc) install httpd as below.

    $yum install httpd

    This will install httpd in your machine and created the related  conf files under /etc/httpd
    
    $service httpd start

* Create a simple home page at /home/vagrant/html/index.html.

+ Change the DocumentRoot path to "/home/vagrant".The can be done by commenting out all lines in
 /etc/httpd/conf.d/welcome.conf and change the below line in /etc/httpd/conf/httpd.conf.

    DocumentRoot "/home/vagrant/html" 

 *The default document root for httpd is : /var/www/html* 
 *The default home page is at :/var/www/html/index.html*. 

* Now restart httpd service.

    $service httpd restart.


On trying to access the page from browser http://my.vm.ip, it gave "403 Forbidden" error as below:

---------------------------------
![screenshot1](/images/scs-3.png)
---------------------------------

I looked at logs in  /var/log/http/error_log and found  the below error:

![screenshot2](/images/scs-2.png)

This is because httpd is one of the system processed which are under targetted SELinux policy.In order to get more idea, let us see the SELinux context for the default DocumentRoot for httpd (/var/www/html)

![screenshot3](/images/scs-1.png)

**The -Z option can be used with most cli options to get the SELinux context modes (e.g: ls -Z, ps -axZ)**

SELinux context for Apache/httpd process is:

![screenshot4](/images/scs-5.png)

The SELinux context for our index.html (/home/vagrant/html/index.html) can be seen as below:

![screenshot5](/images/scs-4.png)


Access can only be allowed for similar types.So we can see that httpd with context type **httpd_t** can access its default homepage (/var/www/html/index.html) with context type **httpd_sys_content_t** because both belong to the same domain.
Whereas  the selinux context type for /home/vagrant/html/index.html (user_home_t) is not http_t , So httpd gets permission denied on trying to access index.html in /home/vagrant/html though the file is readable .


This can be solved by either relabeling the SELinux context on the desired directory that we want httpd to access.

The cli utility to do that is "chcon". For example: changing context for index.html:

![screenshot6](/images/scs-6.png)

![screenshot7](/images/scs-7.png)

You can use Audit2Allow tool to write SElinux policies.The log messages in /var/log/audit/audit.log are audit2allow friendly.

More details on how to use audit2allow can be found on <http://wiki.centos.org/HowTos/SELinux>


Thats all for today.Have a nice time with SELinux !!!










