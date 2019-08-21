Get Started
===========

Lab environment
---------------

- This lab is executed on `Cisco's dCloud environment <https://dcloud.cisco.com>`_

- Once logged in, click on My Hub and view the sessions available to you

  .. image:: ./_static/dcloud_login1.png

..
  
- For the session "name of session" click on green 'View' button

- Click on 'Details' a pop up windows with Session Details will show up. Scroll below on the session details.
  
  .. image:: ./_static/dcloud_login2.png

..
  
- Locate the AnyConnect Credentials and use AnyConnect on your local device to connect to the lab. Your credentials will defer from what is mentioned in the screen shot

- Once VPN connection is sucessful then RDP to the windows client. 

- Back to the Details section on the main session page you will see the topology with various end points. Hover over each end point to locate the login credentials
  
  - Hover over the wkst1 icon to get login credentails of the windows remote desktop

    |
	
    .. image:: ./_static/dcloud_login3.png

    |
	
  - From your local device RDP to the windows workstation
  
  - Once RDP session is successful the lab exercises will be executed from the RDP session
 
For guidance: IP and credentials are mentioned below but each of the components below has a shortcut located 
on the RDP client

=========== ================ ========================== =======================================
Name        IP               Credentials                Remarks                                      
=========== ================ ========================== =======================================
RDP client  198.18.133.36    username: dcloud\\demouser
                             password: C1sco12345
							
BIG-IP	    198.18.128.130   admin/admin     	        Bookmark in RDP browser

APIC        198.18.133.200   admin/C1sco12345	        Bookmark in RDP browser

Linux Host  198.18.134.150   root/C1sco12345		    Session saved in Putty
                
=========== ================ ========================== =======================================

The next few sections will go over the following in sequence

- What L2-L3 APIC configuration is pre-configured and what L4-L7 configuration needs to be done

- Navigate the F5 ACI ServiceCenter application and work through the different use cases

- Look at how we can use API's using POSTMAN to execute workflows

- Use Ansible playbooks to automate tasks using F5 ACI ServiceCenter

- Look at different F5 deployment options