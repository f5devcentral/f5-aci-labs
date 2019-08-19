Lab environment
===============

- This lab is executed on `Cisco's dCloud environment <https://dcloud.cisco.com>`_

- Once logged in, click on My Hub and view the sessions available to you

  |
  
  .. image:: ./_static/dcloud_login1.png

  |
  
- For the session "name of session" click on green 'View' button

- Click on 'Details' a pop up windows with Session Details will show up. Scroll below on the session details.
  
  |
  
  .. image:: ./_static/dcloud_login2.png
  
  |
  
- Locate the AnyConnect Credentials and use AnyConnect on your local device to connect to the lab. Your credentials will defer from what is mentioned in the screen shot

- Once VPN connection is sucessful RDP to the windows client. Back to the Details section on the main session page
  you will see the topology with various end points. Hover over each end point to locate the login credentials
  
  - Hover/click over the wkst1 icon to get login credentails of the windows remote desktop
    
	|
	
    .. image:: ./_static/dcloud_login3.png
  
    |
   
  - From your local device RDP to the windows workstation
  
  - Once RDP session is successful the lab exercises will be executed from the RDP session
 
For guidance: IP and credentials are mentioned below but each of the components below has a shortcut located 
on the RDP client

============= ================ =========================== =========================================
Name          IP               Credentials                 Remarks                                      
============= ================ =========================== =========================================
RDP client    192.18.133.36    username: dcloud\\demouser
                               password: C1sco12345
							   
BIG-IP	      192.18.128.130   admin/admin     	           Bookmark in RDP browser

APIC          192.18.133.200   admin/C1sco12345	           Bookmark in RDP browser

Ansible Tower 192.18.134.150   admin/C1sco12345		       Bookmark in RDP Browser 'Ansible AWX'                
============= ================ =========================== =========================================