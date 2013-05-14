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
-----------------------------
***sestatus:*** command to view the current SELinux status

***setenforce:*** command to switch between permissive and enforcing mode through cli.

*But the changes wont exist on system reboot.Inorder to make the change permanent edit the file */etc/selinux/config* *