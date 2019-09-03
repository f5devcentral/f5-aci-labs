Ansible, F5 and Cisco ACI lab Guide
===================================

This lab guide will cover on how to use Ansible to automate a F5 BIG-IP and Cisco ACI environment

Goal is to use Ansible to automate an end-to-end workflow which can be broken down into following tasks:

- Perform L2-L3 stitching between the Cisco ACI fabric and F5 BIG-IP

- Configure the network on the BIG-IP

- Deploy an application on BIG-IP

- Automate elastic workload commision/decommission

We will be using Ansible Tower to execute all the tasks. 

If new to tower please watch the 10 minute overview before proceeding: https://www.ansible.com/products/tower

|

|
	
.. toctree::
   :caption: Autommate F5 and Cisco ACI using Ansible
   :glob:
   :maxdepth: 2
   :hidden:

   login.rst
   ansible_tower_walkthrough.rst
   dynamic_ep.rst
   troubleshoot.rst