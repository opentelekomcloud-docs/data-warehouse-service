:original_name: dws_03_2132.html

.. _dws_03_2132:

How Applications Communicate with DWS?
======================================

To communicate applications with DWS, make sure the networks between them are connected. The following table lists common connection scenarios.

.. table:: **Table 1** Communication between applications and DWS

   +--------------------------+---------------------------------------------------------------------------------------------------------------------------------------------------+-------------------------------------------------------------------------------------------------------------------------------------------------+---------------------------+
   | Scenario                 |                                                                                                                                                   | Description                                                                                                                                     | Supported Connection Type |
   +==========================+===================================================================================================================================================+=================================================================================================================================================+===========================+
   | Cloud                    | :ref:`Application and DWS Are in the Same VPC in the Same Region <en-us_topic_0000001384777037__section33446188552>`                              | Two private IP addresses in the same VPC can directly communicate with each other.                                                              | -  gsql                   |
   |                          |                                                                                                                                                   |                                                                                                                                                 | -  Data Studio            |
   |                          |                                                                                                                                                   |                                                                                                                                                 | -  JDBC/ODBC              |
   +--------------------------+---------------------------------------------------------------------------------------------------------------------------------------------------+-------------------------------------------------------------------------------------------------------------------------------------------------+---------------------------+
   |                          | :ref:`Applications and DWS Are in Different VPCs in the Same Region <en-us_topic_0000001384777037__section16436174022710>`                        | After a **VPC peering** connection is created between two VPCs, the two private IP addresses can directly communicate with each other.          |                           |
   +--------------------------+---------------------------------------------------------------------------------------------------------------------------------------------------+-------------------------------------------------------------------------------------------------------------------------------------------------+---------------------------+
   |                          | :ref:`Applications and DWS Are in Different Regions <en-us_topic_0000001384777037__section746241873814>`                                          | After a **cloud connection** (CC) is established between two regions, the two regions communicate with each other through private IP addresses. |                           |
   +--------------------------+---------------------------------------------------------------------------------------------------------------------------------------------------+-------------------------------------------------------------------------------------------------------------------------------------------------+---------------------------+
   | On-premises and on-cloud | :ref:`Applications are deployed in on-premise data centers and need to communicate with DWS. <en-us_topic_0000001384777037__section523413148620>` | -  Use the **public IP address** of DWS for communication.                                                                                      |                           |
   |                          |                                                                                                                                                   | -  Use **Direct Connect** (DC) for communication.                                                                                               |                           |
   +--------------------------+---------------------------------------------------------------------------------------------------------------------------------------------------+-------------------------------------------------------------------------------------------------------------------------------------------------+---------------------------+

.. _en-us_topic_0000001384777037__section33446188552:

Application and DWS Are in the Same VPC in the Same Region
----------------------------------------------------------

To ensure low latency, you are advised to deploy applications and DWS in the same region. For example, if an application is deployed on an ECS, you are advised to deploy the DWS cluster in the same VPC as the ECS. In this way, the application can directly communicate with DWS through a private IP address. In this case, deploy the data warehouse cluster in the same region and VPC where the ECS resides.

For example, if the ECS is deployed in , select for the DWS cluster and ensure that the DWS cluster and the ECS are both in **VPC1**. The private IP address of the ECS is **192.168.120.1**, the private IP address of DWS is **192.168.120.2**. Therefore, they can communicate with each other through private IP addresses.

Perform the following operations to check the ECS outbound rules and DWS inbound rules:

#. **Check the ECS outbound rules:**

   Ensure that the outbound rule of the ECS security group allows access. If access is not allowed, see .

   |image1|

#. **Check the DWS inbound rules:**

   If no security group is configured when DWS is created, the default inbound rule allows TCP access from all IPv4 addresses and port 8000. To ensure security, you can also allow only one IP address. For details, see

   |image2|

#. Log in to the ECS. If the private IP address of DWS can be pinged, the network connection is normal. If the IP address cannot be pinged, check the preceding configuration. If the ECS has a firewall, check the firewall configuration.

**Example of using gsql for connection:**

**gsql -d gaussdb -h** *192.168.120.2* **-p 8000 -U dbadmin -W** *password* **-r**

.. _en-us_topic_0000001384777037__section16436174022710:

Applications and DWS Are in Different VPCs in the Same Region
-------------------------------------------------------------

To ensure low latency, you are advised to deploy applications and DWS in the same region. For example, if applications are deployed on an ECS, you are advised to deploy the DWS cluster in the same VPC as the ECS. If a different VPC is selected for the DWS cluster, the ECS cannot directly connect to DWS.

For example, both ECS and DWS are deployed in , but ECS is in **VPC1** and DWS is in **VPC2**. In this case, you need to create a between **VPC1** and **VPC2** so that ECS can access DWS using the private IP address of DWS.

Perform the following operations to check ECS outbound rules, DWS inbound rules, and VPC peering connection between the two VPCs.

#. **Check the ECS outbound rules:**

   Ensure that the outbound rule of the ECS security group allows access. If access is not allowed, see .

   |image3|

#. **Check the DWS inbound rules:**

   If no security group is configured when DWS is created, the default inbound rule allows TCP access from all IPv4 addresses and port 8000. To ensure security, you can also allow only one IP address. For details, see

   |image4|

#. Create a between **VPC1** where the ECS is and **VPC2** where DWS is.

#. Log in to the ECS. If the private IP address of DWS can be pinged, the network connection is normal. If the IP address cannot be pinged, check the preceding configuration. If the ECS has a firewall, check the firewall configuration.

**Example of using gsql for connection:**

**gsql -d gaussdb -h** *192.168.120.2* **-p 8000 -U dbadmin -W** *password* **-r**

.. _en-us_topic_0000001384777037__section746241873814:

Applications and DWS Are in Different Regions
---------------------------------------------

If the application and DWS are in different regions, for example, ECS is in and DWS is in , you need to establish a between the two regions for communication.

.. _en-us_topic_0000001384777037__section523413148620:

Applications are deployed in on-premise data centers and need to communicate with DWS.
--------------------------------------------------------------------------------------

If applications are not on the cloud but in the local data center, they need to communicate with DWS on the cloud.

-  **Scenario 1**: On-premises applications communicate with DWS through DWS public IP addresses.

   Example of using gsql for connection:

   **gsql -d gaussdb -h** *public_IP_address* **-p 8000 -U dbadmin -W** *password* **-r**

-  **Scenario 2**: On-premises applications cannot access the external network. In this case, use .

.. |image1| image:: /_static/images/en-us_image_0000001389594973.png
.. |image2| image:: /_static/images/en-us_image_0000001389237417.png
.. |image3| image:: /_static/images/en-us_image_0000001389597281.png
.. |image4| image:: /_static/images/en-us_image_0000001339077346.png
