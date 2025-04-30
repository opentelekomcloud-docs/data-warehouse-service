:original_name: dws_03_2132.html

.. _dws_03_2132:

How Applications Communicate with GaussDB(DWS)?
===============================================

For applications to communicate with GaussDB(DWS), make sure the networks between them are connected. The following table lists common connection scenarios.

.. table:: **Table 1** Communication between applications and GaussDB(DWS)

   +--------------------------+--------------------------------------------------------------------------------------------------------------------------------------------------------------------+-------------------------------------------------------------------------------------------------------------------------------------------------+---------------------------+
   | Scenario                 |                                                                                                                                                                    | Description                                                                                                                                     | Supported Connection Type |
   +==========================+====================================================================================================================================================================+=================================================================================================================================================+===========================+
   | Cloud                    | :ref:`Service Application and GaussDB(DWS) Are in the Same VPC in the Same Region <en-us_topic_0000001384777037__section33446188552>`                              | Two private IP addresses in the same VPC can directly communicate with each other.                                                              | -  gsql                   |
   |                          |                                                                                                                                                                    |                                                                                                                                                 | -  Data Studio            |
   |                          |                                                                                                                                                                    |                                                                                                                                                 | -  JDBC/ODBC              |
   +--------------------------+--------------------------------------------------------------------------------------------------------------------------------------------------------------------+-------------------------------------------------------------------------------------------------------------------------------------------------+---------------------------+
   |                          | :ref:`Service Applications and GaussDB(DWS) Are in Different VPCs in the Same Region <en-us_topic_0000001384777037__section16436174022710>`                        | After a **VPC peering** connection is created between two VPCs, the two private IP addresses can directly communicate with each other.          |                           |
   +--------------------------+--------------------------------------------------------------------------------------------------------------------------------------------------------------------+-------------------------------------------------------------------------------------------------------------------------------------------------+---------------------------+
   |                          | :ref:`Service Applications and GaussDB(DWS) Are in Different Regions <en-us_topic_0000001384777037__section746241873814>`                                          | After a **cloud connection** (CC) is established between two regions, the two regions communicate with each other through private IP addresses. |                           |
   +--------------------------+--------------------------------------------------------------------------------------------------------------------------------------------------------------------+-------------------------------------------------------------------------------------------------------------------------------------------------+---------------------------+
   | On-premises and on-cloud | :ref:`Service applications are deployed in on-premise data centers and need to communicate with GaussDB(DWS). <en-us_topic_0000001384777037__section523413148620>` | -  Use the **public IP address** of GaussDB(DWS) for communication.                                                                             |                           |
   |                          |                                                                                                                                                                    | -  Use **Direct Connect** (DC) is for communication.                                                                                            |                           |
   +--------------------------+--------------------------------------------------------------------------------------------------------------------------------------------------------------------+-------------------------------------------------------------------------------------------------------------------------------------------------+---------------------------+

.. _en-us_topic_0000001384777037__section33446188552:

Service Application and GaussDB(DWS) Are in the Same VPC in the Same Region
---------------------------------------------------------------------------

To ensure low service latency, you are advised to deploy service applications and GaussDB(DWS) in the same region. For example, if a service application is deployed on an ECS, you are advised to deploy the data warehouse cluster in the same VPC as the ECS. In this way, the application can directly communicate with GaussDB(DWS) through an intranet IP address. In this case, deploy the data warehouse cluster in the same region and VPC where the ECS resides.

For example, if the ECS is deployed in , select for the GaussDB(DWS) cluster and ensure that the GaussDB(DWS) cluster and the ECS are both in **VPC1**. The private IP address of the ECS is **192.168.120.1**, the private IP address of GaussDB(DWS) is **192.168.120.2**. Therefore, they can communicate with each other through private IP addresses.

The key points in communication check are the ECS outbound rule and GaussDB(DWS) inbound rule. The check procedure is as follows:

#. **Check the ECS outbound rules:**

   Ensure that the outbound rule of the ECS security group allows access. If access is not allowed, see .

   |image1|

#. **Check the GaussDB(DWS) inbound rules:**

   If no security group is configured when GaussDB(DWS) is created, the default inbound rule allows TCP access from all IPv4 addresses and port 8000. To ensure security, you can also allow only one IP address. For details, see

   |image2|

#. Log in to the ECS. If the internal IP address of GaussDB(DWS) can be pinged, the network connection is normal. If the IP address cannot be pinged, check the preceding configuration. If the ECS has a firewall, check the firewall configuration.

**Example of using gsql for connection:**

**gsql -d gaussdb -h** *192.168.120.2* **-p 8000 -U dbadmin -W** *password* **-r**

.. _en-us_topic_0000001384777037__section16436174022710:

Service Applications and GaussDB(DWS) Are in Different VPCs in the Same Region
------------------------------------------------------------------------------

To ensure low service latency, you are advised to deploy service applications and GaussDB(DWS) in the same region. For example, if service applications are deployed on an ECS, you are advised to deploy the data warehouse cluster in the same VPC as the ECS. If a different VPC is selected for the data warehouse cluster, the ECS cannot directly connect to GaussDB(DWS).

For example, both ECS and GaussDB(DWS) are deployed in , but ECS is in VPC 1 and GaussDB(DWS) is in VPC 2. In this case, you need to create a between VPC 1 and VPC 2 so that ECS can access GaussDB(DWS) using the private IP address of GaussDB(DWS).

The key points for checking the communication are the ECS outbound rules, GaussDB(DWS) inbound rules, and VPC peering connection. The check procedure is as follows:

#. **Check the ECS outbound rules:**

   Ensure that the outbound rule of the ECS security group allows access. If access is not allowed, see .

   |image3|

#. **Check the GaussDB(DWS) inbound rules:**

   If no security group is configured when GaussDB(DWS) is created, the default inbound rule allows TCP access from all IPv4 addresses and port 8000. To ensure security, you can also allow only one IP address. For details, see

   |image4|

#. Create a between VPC 1 where the ECS is and VPC 2 where GaussDB(DWS) is.

#. Log in to the ECS. If the internal IP address of GaussDB(DWS) can be pinged, the network connection is normal. If the IP address cannot be pinged, check the preceding configuration. If the ECS has a firewall, check the firewall configuration.

**Example of using gsql for connection:**

**gsql -d gaussdb -h** *192.168.120.2* **-p 8000 -U dbadmin -W** *password* **-r**

.. _en-us_topic_0000001384777037__section746241873814:

Service Applications and GaussDB(DWS) Are in Different Regions
--------------------------------------------------------------

If the service application and GaussDB(DWS) are in different regions, for example, ECS is in and GaussDB(DWS) is in , you need to establish a between the two regions for communication.

.. _en-us_topic_0000001384777037__section523413148620:

Service applications are deployed in on-premise data centers and need to communicate with GaussDB(DWS).
-------------------------------------------------------------------------------------------------------

If service applications are not on the cloud but in the local data center, they need to communicate with GaussDB(DWS) on the cloud.

-  **Scenario 1**: On-premises service applications communicate with GaussDB(DWS) through GaussDB(DWS) public IP addresses.

   Example of using gsql for connection:

   **gsql -d gaussdb -h** *public_IP_address* **-p 8000 -U dbadmin -W** *password* **-r**

-  **Scenario 2**: On-premises services cannot access the external network. In this case, use .

.. |image1| image:: /_static/images/en-us_image_0000001389594973.png
.. |image2| image:: /_static/images/en-us_image_0000001389237417.png
.. |image3| image:: /_static/images/en-us_image_0000001389597281.png
.. |image4| image:: /_static/images/en-us_image_0000001339077346.png
