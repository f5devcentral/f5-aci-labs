Ansible playbooks
=================

In this section we are going to take the scenarios that we executed via POSTMAN and use Ansible playbooks to execute the workflow.

We will run the playbooks throught Ansible CLI to get a good understanding of how the playbooks are written and how to execute them.

To run this section, login to the 'tools' linux server. Open putty which is placed on the desktop. Load the tools server and open

- Login with credentials: admin/C1sco12345

- cd to directory '/root/f5aci_servicecenter_playbooks'

Get health statistics playbook
------------------------------

Files used in this section

**vars.yml**: This file defines all the user defined variables that are going to be used by the get_vip_status playbook

.. code-block:: rst

   ## BIG-IP information ##
   bigip_username: "admin"
   bigip_password: "admin"
   big_ip: "198.18.128.130"

   ## APIC information ##
   apic_username: "admin"
   apic_password: "C1sco12345"
   apic_ip: "198.18.133.200"

   tenant_name: "LAX"
   ldev_name: "BIGIP-VE-Standalone"
   part_name: "DemoPartition"

   ## IP address of docker container running the application ##
   ## This is used only in the case of a APIC simulator ##
   ## This will NOT be needed with an environment with a physical APIC ##
   docker_ip: "198.18.134.150"

**get_vip_status.yml**: This is the main playbook that will be executed. Let's go through a snippet of the playbook. Each task has a decription of the action perfomed by that task

.. code-block:: yaml
     
   ---
   - name: Get Virtual IP health statistics - F5 ServiceCenter
     hosts: localhost
     gather_facts: false
     connection: local

     #Including the variable file 
     vars_files:
      vars.yml

     #Tasks that are going to be executed in order
     tasks:

     #Using the 'uri' module of Ansible. Making a POST call similiar to what we did using POSTMAN
     #Register key in the end will take the output from POST and store it in a variable 'cookie'
     - name: Login to APIC
       uri:
        url: https://{{apic_ip}}/api/aaaLogin.json
        method: POST
        validate_certs: no
        body_format: json
        body:
         aaaUser:
          attributes:
           name: "{{apic_username}}"
           pwd: "{{apic_password}}"
        headers:
        content_type: "application/json"
       return_content: yes
      register: cookie

     #Debug is used to display content
     - debug: msg="{{cookie['cookies']['APIC-cookie']}}"

     #Set fact is used to store a variable that can be used later in the playbook
     - set_fact:
       token: "{{cookie['cookies']['APIC-cookie']}}"
    
.. note ::
	
   This is not the complete playbook in the documentation. Go through the complete playbook before executing it

Execute the playbook by running the following command

- ansible-playbook get_vip_status.yml

Go through the output, you will see that it shows the vip/pool and node information. 

In the last section the task parses the output to display the node member IPs that belong to a particular EPG. If you had workload belonging to different EPG's using this script you can only extract workload information that belong to a particular EPG

In this case all node IP's that belong to EPG 'Provider-EPG'

.. code-block:: RST

   TASK [Give me all Node IP address that belong to EPG Provider-EPG]
   
   ok: [localhost] => (item={u'status': u'unknown', u'name': u'10.193.101.2', u'app': {u'dn': u'uni/tn-LAX/ap-LAX-APN', u'name': u'LAX-APN'}, u'partition': u'DemoPartition', u'enabled': u'enabled', u'address': u'10.193.101.2', u'epg': {u'dn': u'uni/tn-LAX/ap-LAX-APN/epg-Provider-EPG', u'name': u'Provider-EPG'}, u'fullpath': u'/DemoPartition/10.193.101.2', u'tenant': {u'dn': u'uni/tn-LAX', u'name': u'LAX'}}) => {
    "msg": "10.193.101.2" 
   }
   
   ok: [localhost] => (item={u'status': u'unknown', u'name': u'10.193.101.3', u'app': {u'dn': u'uni/tn-LAX/ap-LAX-APN', u'name': u'LAX-APN'}, u'partition': u'DemoPartition', u'enabled': u'enabled', u'address': u'10.193.101.3', u'epg': {u'dn': u'uni/tn-LAX/ap-LAX-APN/epg-Provider-EPG', u'name': u'Provider-EPG'}, u'fullpath': u'/DemoPartition/10.193.101.3', u'tenant': {u'dn': u'uni/tn-LAX', u'name': u'LAX'}}) => {
    "msg": "10.193.101.3"
   }

Right now we have only workload that belong to the EPG of Tenant LAX

Let's add an application on the BIG-IP that contains workload that belongs to Tenant SJC as well.

.. note::
   
   Go to Application Services tab on the F5 ACI ServiceCenter and deploy a new parition and application
   
   Example: 
   
   - Partition Name: "DemoParitionSJC"
   
   - Application Name: "DemoApplicationSJC"
   
   Change the stub code to relect the following
   
   - Virtual IP: 10.10.20.100
   
   - Pool member IPs: 10.193.102.2 and 10.193.102.3 (the difference is the third octect from workload in Tenant LAX)

   Also open POSTMAN go to Collection 'EndPoint Managment', go to request 'Add EndPoint SJC'
   
   - Change the payload to 
   
     - <fvRsPathAtt tDn="topology/pod-1/paths-102/pathep-[eth1/3]" encap="vlan-2003"/> 
	
     - Click send 
   
   - This will add another learned endpoint to the Tenant SJC on the APIC
   
Once you have deployed the configuration, go to the Visibility tab and choose the partition you created, select the VIP table you will see your VIP with the workload belong to SJC Tenant/App/EPG

Now change the **vars.yml** to point to the new Tenant and EPG (EPG name is the same in both tenants)

.. code-block:: rst

   tenant_name: "SJC"
   epg_name: "Provider-EPG"
   part_name: "DemoParitionSJC" <<OR if you provided a different name>>
    
Run your playbook again: 

**ansible-playbook get_vip_status.yml** 

Output will be similiar to

.. code-block:: RST

   TASK [Give me all Node IP address that belong to EPG Provider-EPG]

   ok: [localhost] => (item={u'status': u'unknown', u'name': u'10.193.102.2', u'app': {u'dn': u'uni/tn-SJC/ap-SJC-APN', u'name': u'SJC-APN'}, u'partition': u'DemoPartitionSJC', u'enabled': u'enabled', u'address': u'10.193.102.2', u'epg': {u'dn': u'uni/tn-SJC/ap-SJC-APN/epg-Provider-EPG', u'name': u'Provider-EPG'}, u'fullpath': u'/DemoPartitionSJC/10.193.102.2', u'tenant': {u'dn': u'uni/tn-SJC', u'name': u'SJC'}}) => {
    "msg": "10.193.102.2" 
   }

   ok: [localhost] => (item={u'status': u'unknown', u'name': u'10.193.102.3', u'app': {u'dn': u'uni/tn-SJC/ap-SJC-APN', u'name': u'SJC-APN'}, u'partition': u'DemoPartitionSJC', u'enabled': u'enabled', u'address': u'10.193.102.3', u'epg': {u'dn': u'uni/tn-SJC/ap-SJC-APN/epg-Provider-EPG', u'name': u'Provider-EPG'}, u'fullpath': u'/DemoPartitionSJC/10.193.102.3', u'tenant': {u'dn': u'uni/tn-SJC', u'name': u'SJC'}}) => {
    "msg": "10.193.102.3"
   }

Output will display workload now that belong to APIC tenant SJC and EPG Provider-EPG

This brings us to an end of the lab exercises. To summarize what we went through

- Walkthrough the use cases

  - Visbility
  
  - L2-L3 stitching
  
  - L4-L7 application
  
- Learnt how APIs can be used to interract with the application

- Learnt how ansible can be used to execute the API's and remove relevant information

We will now delete the configuration on the BIG-IP using the F5 ACI ServiceCenter using Ansible playbooks

Delete configuration playbooks
------------------------------

L4-L7 configuration
```````````````````

Let's start be deleting the L4-L7 configuration. We will execute the playbook 'delete_application.yml'

View the code before executing the playbook
  
Command: **ansible-playbook delete.application.yml**

Go back to the F5 ACI ServiceCenter and click on the L4-L7 App services tab.

Try to select a parition, if partition deleted sucessfully then at this point you will see only 1 more partition

Change the vars.yml file to reflect the parition you see and run the ansible playbook again

Go back to the F5 ACI ServiceCenter and click on the L4-L7 App services tab.

Try to select a parition, at the point there should be no paritions and the L4-L7 configuration has been successfully deleted


L2-L3 configuration
```````````````````
Let's delete the L2-L3 configuration. We will execute the playbook 'delete_network.yml'

View the code before executing the playbook
	
Command: **ansible-playbook delete.network.yml**

Go back to the F5 ACI ServiceCenter and click on the L2-L3 stitching tab.

Select the LDEV and then select the VLANS, there will be no configuration.

Also login to the BIG-IP and verify no vlans/self-IP's exist and no parition expect common exists

**This brings us to the end of the section**

Key Takeaways
=============

- Single point of automation with enhanced visibility into Cisco ACI and F5 BIG-IP

- Use cases are NOT dependent on each other

  - Each use case is used to serve a different purpose and are not bound to work togeather. You can use just the visibility tab to troublshoot your network if that works best in your environemnt. OR just use the F5 ACI ServiceCenter to just deploy L4-L7 applications on the BIG-IP.

- Suitable for both greenfield and brownfield deployment
 
  - If BIG-IP network configuration already exists or you are used to managing the BIG-IP with your own tool or manually no problem you can still use visibility to get insight into your network 
  
- F5 ACI App is a stateful app

  - The newtwork or application infomration configured throught the F5 ACI ServiceCenter is stored in the application database and hence you get all to manage the lifecycle of the configuration. Not just perform Day0 tasks but use it for DayN tasks as well

- Deleting F5 ACI App will not impact ACI and BIG-IP configurations
  
  - You can install and delete the application to your convience, deleting the app will not disrupt any configuration on either the APIC or the BIG-IP
  
- F5 ACI App can be automated by APIC REST API

  - If you have an automtion workflow this app will fit right in since there is a REST API endpoint provided for automating all the tasks

- The F5 ACI App is free of charge. Supported by F5
