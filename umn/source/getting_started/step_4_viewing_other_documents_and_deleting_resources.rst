:original_name: dws_01_0079.html

.. _dws_01_0079:

Step 4: Viewing Other Documents and Deleting Resources
======================================================

Viewing Other Relevant Documents
--------------------------------

After performing the steps in preceding sections, you can refer to the documentation listed as follows for more information about GaussDB(DWS):

-  *Data Warehouse Service User Guide*: This guide provides detailed information about the concepts and operations related to creating, managing, monitoring, and connecting to clusters.
-  *Data Warehouse Service Database Development Guide*: This guide provides comprehensive and detailed information for database developers to understand how to build, manage, and query GaussDB(DWS) databases, including SQL syntax, user management, and data import and export.

Deleting Resources
------------------

After performing the steps in "Getting Started," if you do not need to use the sample data, clusters, ECSs, or VPCs, delete the resources so that your service quotas will not be wasted or occupied.

#. Delete the ECS created in :ref:`Step 3: Connecting to a Cluster <en-us_topic_0000001707254513>` for connecting to the GaussDB(DWS) cluster.

   a. Log in to the cloud console.
   b. Click **Service List** and choose **Computing** > **Elastic Cloud Server** to enter the ECS console.
   c. In the navigation tree on the left, click **Elastic Cloud Server**. In the ECS list, select the ECS to be deleted and click **Delete**. You can choose to delete the associated EIP and data disks at the same time. If you do not delete them, they are reserved. If necessary, you can manually delete them later.
   d. Click **OK**.

#. Delete a GaussDB(DWS) cluster.

   On the GaussDB(DWS) console, click **Clusters** > **Dedicated Clusters**, locate the row that contains **dws-demo** in the cluster list, and choose **More** > **Delete**. In the dialog box that is displayed, select **Release the EIP bound with the cluster** and click **OK**.

   If the cluster to be deleted uses an automatically created security group that is not used by other clusters, the security group is automatically deleted when the cluster is deleted.

#. Delete a subnet. Before deleting the subnet, ensure that it is not bound to other resources.

   Log in to the VPC management console. In the navigation tree on the left, click **Virtual Private Cloud**. In the VPC list, click **vpc-dws**. In the subnet list, locate the row that contains **subnet-dws** and click **Delete**.

#. Delete a VPC. Before deleting the VPC, ensure that it is not bound to other resources.

   Log in to the VPC management console, locate the row that contains **vpc-dws** in the VPC list, and click **Delete**.

   For details, see "VPC and Subnet > Deleting a VPC" in the *Virtual Private Cloud User Guide*.
