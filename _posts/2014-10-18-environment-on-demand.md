---
layout: post
title: "Environment On Demand"
description: ""
category: 
tags: []
---
{% include JB/setup %}

We all know how important it is to have automated environment in your project. In our regular work we speak about automated build and deployment in enterprises, very less is spoken about or given important to the initial setup - provisioning the virtual environments. Automated provisioning of environment on demand is equally important as is automated build and deployment. It is the entire workflow - Automated Environment on Demand => Config Management => Automated Build and Deployment (CI)

As part of an in house initiative, we started building a POC of Environment on Demand using Openstack .The technical stack we used are :

Hardware:

5 machines - each with 8 GB ram and 250 GB disk space

Which consists of :

1.Windows Server 2008 R2 - Vcenter Server
2.Vmware Exsi 5.5 - VMware Hosts (2)
3.Centos 6.5 - KVM Server
4.Centos 6.5 + Openstack RDO (Redhat Openstack) - Openstack server

Using the above infrastructure we had setup an openstack-automated environment with VMware and KVM as the hypervisors. Below diagram is an overview of the setup.

---------------------------------
![screenshot1](/images/Slide1.jpg)
---------------------------------
