:original_name: dws_01_0822.html

.. _dws_01_0822:

Associating and Disassociating ELB
==================================

Overview
--------

If the private IP address or EIP of a CN is used to connect to a cluster, the failure of this CN will lead to cluster connection failure. If a private domain name is used for connection, the DNS service randomly selects a private IP address or EIP for each client. This cannot balance loads or avoid single-CN failures. ELB is used to solve these problems.

An ELB distributes access traffic to multiple ECSs for traffic control based on forwarding policies. It improves the fault tolerance capability of application programs. For details, see *Elastic Load Balance User Guide*.

With ELB health checks, CN requests of a cluster can be quickly forwarded to normal CNs. If a CN is faulty, the workload can be immediately shifted to a healthy node, minimizing cluster access faults.

.. note::

   -  This feature is supported only in cluster version 8.1.1.200 or later.
   -  For load balancing and high availability purposes, and to prevent single CN failures, a cluster must be bound to ELB.
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
-  When you disassociate ELB from a cluster, related cluster information is cleared on GaussDB(DWS) but the load balancer is not deleted.
-  If you need to access the ELB cluster using a public IP address or domain name, bind an EIP or domain name on the ELB management console.

Associating ELB
---------------

#. Log in to the GaussDB(DWS) console.

#. Choose **Clusters** > **Dedicated Clusters**. All clusters are displayed by default.

#. In the cluster list, click the name of the target cluster. The **Cluster Information** page is displayed.

#. On the **Basic Information** page that is displayed, click **Associate ELB** and select the ELB name. If no load balancer exists, create one on the ELB management console. Then refresh the GaussDB(DWS) page and associate ELB with the cluster.

#. After the request is delivered, go back to the **Clusters** page. Task information **Associating ELB** of the cluster is displayed. The process takes some time.

#. Log in to the ELB management console, choose **Elastic Load Balance** > **Load Balancers**, click the name of the bound load balancer, switch to the **Backend Server Groups** tab, and check whether the cluster CNs are associated with the load balancer.

   |image1|

7. In the **Basic Information** area of the **Cluster Information** page, check the **ELB Address**, which is used for connecting to the cluster.

Disassociating ELB
------------------

#. Log in to the GaussDB(DWS) console.
#. Choose **Clusters** > **Dedicated Clusters**. All clusters are displayed by default.
#. In the cluster list, click the name of the target cluster. The **Cluster Information** page is displayed.
#. On the **Basic Information** page that is displayed, click **Disassociate ELB**.
#. After the request is delivered, go back to the **Clusters** page. Task information **Dissociating ELB** of the cluster is displayed. The process takes some time.
#. Log in to the ELB management console, click the name of the dissociated ELB, switch to the **Backend Server Groups** tab, and check whether the cluster CNs are deleted.

.. |image1| image:: /_static/images/en-us_image_0000001924728940.png
