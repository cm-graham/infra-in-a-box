# Introduction

## Prerequisites

+ Linux (currently designed for Ubuntu 16.04 LTS host)
+ Host system
  + Quad-core CPU
  + 16GB RAM
  + 500G free space
  + For initial setup, an Internet connection
  + If managing both VMs and bare-metal, 2 NICs - external and internal
+ Installed packages
  + [Vagrant](https://vagrantup.com)
  + [VirtualBox](https://virtualbox.org/) with Extension Pack installed
  + [Ansible](https://www.ansible.com/)
+ Git clone of these files

## Summary

The idea behind this is to be able to to build a system that can bootstrap a
data center, or alternatively, to act as a standalone master where necessary.
The end goal is to have everything needed to manage
[Kubernetes](https://kubernetes.io)'s minions from a powered off state running
in a single machine.

## Architecture

To accomplish this the architecture uses Vagrant and VirtualBox to automate the
creation and standing up of virtual machines for all of the management systems
needed to run a Kubernetes cluster. Ansible is used to do the initial
configuration of the systems.

The reasoning behind running all the management systems in virtual machines is
to give a consistent setup regardless if this is stood up on a host running a
GUI or not.

# Setup

# References

+ Loosely based on
  [vagrant-maas-in-a-box](https://github.com/battlemidget/vagrant-maas-in-a-box)

# To do

+ Determine best way to make the systems' networking configuration a variable
  both for Vagrant and subsequently for the Ansible playbook.
+ Determine a way of making user names to the Ansible playbook.

# Future work - may or may never be done!

+ Create a smaller Vagrant box image, geerlingguy/ubuntu1604 uses an 80GB disk.
  This isn't urgent as disk usage is  dynamic.
+ Script to convert between VM only and VM + Bare Metal setups. This should be a
  simple matter of changing the 3rd NIC to be bridged or private (in the
  case of VirtualBox using virtualbox__intnet to name the internal network).
+ Make more cross-distribution friendly, at least RedHat / CentOS and Debian,
  as well as maybe CoreOS, etc. Should be as release agnostic as possible,
  too. The big win, would be able to use this on a Windows host, that requires
  being able to start VMs remotely from the MaaS server.
+ Make it cross-virtualization tech friendly, at least for QEMU and VMWare.
+ It would be preferred if the management services could all be running in
  containers. However, currently some services are difficult to configure to
  achieve that. For instance, juju would need to handle being bootstrapped into
  an LXD and requesting machines from MaaS.
