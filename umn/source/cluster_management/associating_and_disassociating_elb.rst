:original_name: dws_01_0822.html

.. _dws_01_0822:

Associating and Disassociating ELB
==================================

Overview
--------

If the internal IP address or EIP of a CN is used to connect to a cluster, the failure of this CN will lead to cluster connection failure. If a private domain name is used for connection, connection failures can be avoided by polling. However, private domain names cannot be used for public network access, and requests cannot be forwarded in the case of a CN failure. Therefore, ELB is used to avoid single CN failures.

An ELB distributes access traffic to multiple ECSs for traffic control based on forwarding policies. It improves the fault tolerance capability of application programs. For details, see *Elastic Load Balance User Guide*.

With ELB health checks, CN requests of a cluster can be quickly forwarded to normal CNs. If a CN is faulty, the workload can be immediately shifted to a healthy node, minimizing cluster access faults. Currently, ELBs can be bound in the same VPC or across VPCs.

.. note::

   -  This feature is supported only in cluster version 8.1.1.200 or later.
   -  For load balancing and high availability purposes, and to prevent single CN failures, a cluster must be bound to ELB.
   -  When you bind a cluster to ELB across VPCs, you can bind it to a dedicated load balancer.
   -  ELB does not support cross-database access.

Constraints and Limitations
---------------------------

-  To bind an ELB to a GaussDB(DWS) cluster, the ELB must be in the same region, VPC, and enterprise project as the cluster.
-  Only dedicated load balancers can be bound to GaussDB(DWS).

   .. important::

      Load balancing is not supported in regions where the dedicated load balancer is not available. You can check whether dedicated load balancers are supported on the ELB console.

-  The ELB to be associated must use TCP and has a private IP address.
-  When creating an ELB instance, determine its specifications based on your service access traffic. You are advised to select the maximum specifications. On the GaussDB(DWS) console, you can bind to an ELB instance but cannot change its specifications.
-  You only need to create a load balancer if you want to use ELB. GaussDB(DWS) automatically creates the required ELB listeners and backend server groups.
-  When creating a load balancer, ensure that the listeners do not use the same port as the database. Otherwise, ELB cannot be associated.
-  When you associate ELB, the **ROUND_ROBIN** policy is set by default. In addition, the health check interval is set to 10 seconds, the timeout duration is set to 50 seconds, and the number of maximum retries is set to 3. Exercise caution when you modify these ELB parameters.
-  When you disassociate ELB from a cluster, related cluster information is cleared on GaussDB(DWS) but the load balancer is not deleted. Delete the load balancer in time to prevent unnecessary costs.
-  If you need to access the ELB cluster using a public IP address or domain name, bind an EIP or domain name on the ELB management console.

.. _en-us_topic_0000001517754165__section14490205551611:

Associating ELB
---------------

#. Log in to the GaussDB(DWS) management console.

#. Click **Clusters**. All clusters are displayed by default.

#. In the cluster list, click the name of the target cluster. The **Cluster Information** page is displayed.

#. On the **Basic Information** page that is displayed, click **Associate ELB** and select the ELB name. If no load balancer exists, create one on the ELB management console. Then refresh the GaussDB(DWS) page and associate ELB with the cluster.

#. After the request is delivered, go back to the **Clusters** page. Task information **Associating ELB** of the cluster is displayed. The process takes some time.

   |image1|

#. Log in to the ELB management console, click the name of the associated ELB, switch to the **Backend Server Groups** tab, and check whether the cluster CNs are associated with the load balancer.

   |image2|

7. In the **Basic Information** area of the **Cluster Information** page, check the **ELB Address**, which is used for connecting to the cluster.

   |image3|

Prerequisites for Binding an ELB to a Cluster Across VPCs
---------------------------------------------------------

**Enabling ELB for a cross-VPC backend server**

#. Log in to the ELB console.

#. In the ELB list, click the name of a dedicated network ELB to go to its details page.

   |image4|

#. On the **Summary** page, enable **IP as a Backend**, confirm the information, and click **OK**.

   |image5|

   |image6|

#. Check the VPC and subnet segment.

   |image7|

**Connecting the cluster VPC and the ELB VPC (by using VPC peering as an example)**

#. Log in to the GaussDB(DWS) console.

#. Click **Clusters**. All clusters are displayed by default.

#. In the cluster list, click the name of a cluster to go to the cluster details page. Check the VPC and subnet segment of the cluster.

   |image8|

#. Log in to the VPC management console. choose **My VPCs** in the navigation pane, and locate the VPC for which you want to create a VPC peering connection.

   |image9|

#. Choose **VPC Peering Connections**. In the upper right corner of the page, click **Create VPC Peering Connection**.

   |image10|

#. On the displayed page, set **Local VPC** to the cluster VPC, and set **Peer VPC** to the VPC of the ELB. Confirm the settings and click **OK**.

   |image11|

#. Click **Add Route** to add the route information.

   |image12|

#. Click the name of the created VPC peering connection. On the displayed page, click the **Local Routes** tab, click **Route Tables**, and add the route table of the cluster VPC.

   |image13|

#. In the local route table, set **Destination** to the subnet CIDR block of the ELB VPC, set **Next Hop Type** to **VPC peering connection**, and set **Next Hop** to the created VPC peering connection. Click **OK**.

   |image14|

#. Go to the basic information page of the created VPC peering connection, click the **Peer Routes** tab, click **Route Tables**, and add the route table of the ELB VPC.

   |image15|

#. In the peer route table, set **Destination** to the subnet CIDR block of the cluster VPC, set **Next Hop Type** to **VPC peering connection**, and set **Next Hop** to the created VPC peering connection. Click **OK**.

   |image16|

#. After the cluster is created, the network between the VPC where the cluster resides and the VPC where the load balancer resides is connected. For details, see section :ref:`Binding an ELB <en-us_topic_0000001517754165__section14490205551611>`.

Disassociating ELB
------------------

#. Log in to the GaussDB(DWS) management console.

#. Click **Clusters**. All clusters are displayed by default.

#. In the cluster list, click the name of the target cluster. The **Cluster Information** page is displayed.

#. On the **Basic Information** page that is displayed, click **Disassociate ELB**.

   |image17|

#. After the request is delivered, go back to the **Clusters** page. Task information **Dissociating ELB** of the cluster is displayed. The process takes some time.

   |image18|

#. Log in to the ELB management console, click the name of the dissociated ELB, switch to the **Backend Server Groups** tab, and check whether the cluster CNs are deleted.

   |image19|

.. |image1| image:: /_static/images/en-us_image_0000001686967957.png
.. |image2| image:: /_static/images/en-us_image_0000001467074090.png
.. |image3| image:: /_static/images/en-us_image_0000001517355285.png
.. |image4| image:: /_static/images/en-us_image_0000001517440912.png
.. |image5| image:: /_static/images/en-us_image_0000001517600868.png
.. |image6| image:: /_static/images/en-us_image_0000001568200921.png
.. |image7| image:: /_static/images/en-us_image_0000001517281352.png
.. |image8| image:: /_static/images/en-us_image_0000001686971601.png
.. |image9| image:: /_static/images/en-us_image_0000001568400821.png
.. |image10| image:: /_static/images/en-us_image_0000001517121768.png
.. |image11| image:: /_static/images/en-us_image_0000001568081185.png
.. |image12| image:: /_static/images/en-us_image_0000001517440916.png
.. |image13| image:: /_static/images/en-us_image_0000001517600872.png
.. |image14| image:: /_static/images/en-us_image_0000001568200925.png
.. |image15| image:: /_static/images/en-us_image_0000001517281356.png
.. |image16| image:: /_static/images/en-us_image_0000001568281077.png
.. |image17| image:: /_static/images/en-us_image_0000001638868064.png
.. |image18| image:: /_static/images/en-us_image_0000001687029873.png
.. |image19| image:: /_static/images/en-us_image_0000001466594958.png
