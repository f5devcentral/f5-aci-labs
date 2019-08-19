F5 ACI Labs 
===========

Welcome
-------

Welcome to the labs for F5's ACI ServiceCenter app.

Below lab(s) are executed using Cisco's dCloud environment. You will need a Cisco CCO account to have access to the below session

Cisco and F5 dCloud Lab Guide
-----------------------------

Cisco dCloud is a fully scripted, customizable environments available almost instantly in the cloud for free. It has a huge catalog of demos, training and sandboxes for every Cisco architecture.

F5 has utilized this dCloud environment to build two solutions

- Integration between F5 and Cisco ACI using F5 ACI ServiceCenter

- Automating end to end workflow on F5 and ACI using Ansible

Below are the two lab guides for each solution. If you want to run both the labs execute each lab on a fresh dcloud session since there might be stale configuration after running one lab which can interfere with the other lab

.. note::

   The two guides use the same dCloud session "<<Give name>>"
   
   Suggestion is to run each lab in a new dCloud session and not use the same session to run both the labs 

	
.. toctree::
   :caption: Cisco and F5 - dCloud Lab Guides
   :maxdepth: 3

   service_center/index.rst
   pure_ansible/index.rst