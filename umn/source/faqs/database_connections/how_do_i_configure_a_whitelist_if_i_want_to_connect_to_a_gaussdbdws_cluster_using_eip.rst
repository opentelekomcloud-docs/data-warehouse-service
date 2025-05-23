:original_name: dws_03_2130.html

.. _dws_03_2130:

How Do I Configure a Whitelist If I Want to Connect to a GaussDB(DWS) Cluster Using EIP?
========================================================================================

You can log in to the VPC management console to manually create a security group. Then, return to the GaussDB(DWS) cluster creation page, click |image1| next to the **Security Group** drop-down list to refresh the page, and select the newly created security group.

To enable the GaussDB(DWS) client to connect to the cluster, add an inbound rule to the new security group to allow access to the GaussDB(DWS) cluster's database port.

-  **Protocol**: **TCP**

-  **Port**: **8000** Use the database port set when creating the GaussDB(DWS) cluster. This port receives client connections to GaussDB(DWS).

-  **Source**: Select **IP address** and use the host IP address of the client host, for example, **192.168.0.10/32**.


   .. figure:: /_static/images/en-us_image_0000001330488876.png
      :alt: **Figure 1** Adding an inbound rule

      **Figure 1** Adding an inbound rule

   The whitelist will be added.

.. |image1| image:: /_static/images/en-us_image_0000001381609449.png
