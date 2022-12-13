:original_name: dws_03_2130.html

.. _dws_03_2130:

How Do I Configure a Whitelist to Protect Clusters Available Through a Public IP Address?
=========================================================================================

You can also log in to the VPC management console to manually create a security group. Then, go back to the page for creating data warehouse clusters, click the |image1| button next to the **Security Group** drop-down list to refresh the page, and select the new security group.

To enable the GaussDB(DWS) client to connect to the cluster, you need to add an inbound rule to the new security group to grant the access permission to the database port of the GaussDB(DWS) cluster.

-  **Protocol**: **TCP**

-  **Port**: **8000** Use the database port set when creating the GaussDB(DWS) cluster. This port is used for receiving client connections to GaussDB(DWS).

-  **Source**: Select **IP address** and use the host IP address of the client host, for example, **192.168.0.10/32**.

   The whitelist will be added.

.. |image1| image:: /_static/images/en-us_image_0000001196492200.png
