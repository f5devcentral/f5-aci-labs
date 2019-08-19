Enable the F5 ACI ServiceCenter
===============================

The APIC used here is a simulator and an ACI App cannot be loaded on the simulator.

In this section we are going to login to the linux host start the F5 ACI ServiceCenter which is present as a docker container. 

.. note::

   In an environment where there is a physical APIC (standalone or cluster), the F5 ACI ServiceCenter will be present as a software image that is uploaded to the APIC
   Access to the application will from within the APIC user itself. 
   
   View this video to familarize yourself with the APIC user interface and where the application will be accessed from.
   
   **In this lab environment we are using a APIC simulator, so the F5 ACI ServiceCenter will be accessed from outside of the APIC**
   
**Lets begin**

- In the RDP session click on the Putty icon which is present in the toolbar

- Select machine 'f5-tools1' and click on the load button. Make sure you have selected the right machine.

  |
  
  .. image:: ./_static/login_putty.png

  |

- Click on the open button. Login with credentials root/C1sco12345

- Run the following commands
  
  .. code-block:: rst
     
	 sudo docker run --net host -d <<name_of_file>>
	 
	 Example:
     
	 sudo docker run --net host -d <<name of file>>
	 
	 PAYAL - get screen shot

- Open windows cmd

  - cd F5Networks_F5ACIServiceCenter/UIAssets/angular
  
  - Run command

    .. code-block:: rst
   
       npm start

  - Wait for the program to completely start

    |
	
    .. image:: ./_static/npm_start.png
	
    |
	
- Access app using URL http://localhost:4200. You will see a login screen as below:

  |
  
  .. image:: ./_static/docker_screen_initial.png
  
  |
  
The docker container for the F5 ACI AppCenter is sucessfully started and is already connected to the APIC simulator in the backend

.. note::

   **Again this method is only for the lab environment, on a physical APIC the F5 ACI ServiceCenter can be uploaded as an application and will be accessed via the APIC UI**

Leave this screen as is for now we will come back to managing the BIG-IP using the F5 ACI ServiceCenter   
   