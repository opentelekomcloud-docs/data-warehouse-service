:original_name: dws_01_0822.html

.. _dws_01_0822:

Associating and Disassociating ELB
==================================

Overview
--------

If the internal IP address or EIP of a CN is used to connect to a cluster, the failure of this CN will lead to cluster connection failure. If a private domain name is used for connection, connection failures can be avoided by polling. However, private domain names cannot be used for public network access, and requests cannot be forwarded in the case of a CN failure. Therefore, ELB is used to avoid single CN failures.

An ELB distributes access traffic to multiple ECSs for traffic control based on forwarding policies. It improves the fault tolerance capability of application programs. For details, see *Elastic Load Balance User Guide*.

With ELB health checks, CN requests of a cluster can be quickly forwarded to normal CNs. If a CN is faulty, the workload can be immediately shifted to a healthy node, minimizing cluster access faults.

The following ELB operations are supported:

-  :ref:`Associating ELB <en-us_topic_0000001189568137__section14490205551611>`
-  :ref:`Disassociating ELB <en-us_topic_0000001189568137__section9928171214196>`

.. note::

   -  This feature is supported only in cluster version 8.1.1 and later.
   -  For load balancing and high availability purposes, and to prevent single CN failures, a cluster must be bound to ELB.

Constraints and Limitations
---------------------------

-  To bind an ELB to a GaussDB(DWS) cluster, the ELB must be in the same region, VPC, and enterprise project as the cluster.
-  Only dedicated load balancers can be bound to GaussDB(DWS).
-  The ELB to be associated must use TCP and has a private IP address.
-  When creating a load balancer, configure the specifications by service access traffic. ELB specifications cannot be modified on the GaussDB(DWS) management console.
-  You only need to create a load balancer if you want to use ELB. GaussDB(DWS) automatically creates the required ELB listeners and backend server groups.
-  When creating a load balancer, ensure that the listeners do not use the same port as the database. Otherwise, ELB cannot be associated.
-  When you associate ELB, the **ROUND_ROBIN** policy is set by default. In addition, the health check interval is set to 10 seconds, the timeout duration is set to 50 seconds, and the number of maximum retries is set to 3. Exercise caution when you modify these ELB parameters.
-  When you disassociate ELB from a cluster, related cluster information is cleared on GaussDB(DWS) but the load balancer is not deleted. Delete the load balancer in time to prevent unnecessary costs.
-  If you need to access the ELB cluster using a public IP address or domain name, bind an EIP or domain name on the ELB management console.

.. _en-us_topic_0000001189568137__section14490205551611:

Associating ELB
---------------

#. Log in to the GaussDB(DWS) management console.

#. Click **Clusters**. All clusters are displayed by default.

#. Click the name of the target cluster.

#. On the **Basic Information** page that is displayed, click **Associate ELB** and select the ELB name. If no load balancer exists, create one on the ELB management console. Then refresh the GaussDB(DWS) page and associate ELB with the cluster.

#. After the request is delivered, go back to the **Clusters** page. Task information **Associating ELB** of the cluster is displayed. The process takes some time.

   |image1|

#. Log in to the ELB management console, click the name of the associated ELB, switch to the **Backend Server Groups** tab, and check whether the cluster CNs are associated with the load balancer.

   |image2|

7. Open the **Basic Information** tab of the cluster. The **ELB Address** will be used for connecting to the cluster.

   |image3|

.. _en-us_topic_0000001189568137__section9928171214196:

Disassociating ELB
------------------

#. Log in to the GaussDB(DWS) management console.

#. Click **Clusters**. All clusters are displayed by default.

#. Click the name of the target cluster.

#. On the **Basic Information** page that is displayed, click **Disassociate ELB**.

   |image4|

#. After the request is delivered, go back to the **Clusters** page. Task information **Dissociating ELB** of the cluster is displayed. The process takes some time.

   |image5|

#. Log in to the ELB management console, click the name of the dissociated ELB, switch to the **Backend Server Groups** tab, and check whether the cluster CNs are deleted.

   |image6|

.. |image1| image:: /_static/images/en-us_image_0000001217284196.png
.. |image2| image:: /_static/images/en-us_image_0000001144491744.png
.. |image3| image:: /_static/images/en-us_image_0000001238779265.png
.. |image4| image:: /_static/images/en-us_image_0000001190291829.png
.. |image5| image:: /_static/images/en-us_image_0000001144332116.png
.. |image6| image:: /_static/images/en-us_image_0000001144332046.png
