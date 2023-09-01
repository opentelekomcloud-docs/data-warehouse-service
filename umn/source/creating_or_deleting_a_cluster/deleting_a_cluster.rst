:original_name: dws_01_0025.html

.. _dws_01_0025:

Deleting a Cluster
==================

If you do not need to use a cluster, perform the operations in this section to delete it.

Impact on the System
--------------------

Deleted clusters cannot be recovered. Additionally, you cannot access user data and automated snapshots in a deleted cluster because the data and snapshots are automatically deleted. If you delete a cluster, its manual snapshots will not be deleted.


Deleting a Cluster
------------------

#. Log in to the GaussDB(DWS) management console.

#. Click |image1| in the upper left corner of the management console to select a region.

#. On the **Clusters** page, locate the cluster to be deleted.

#. In the row of a cluster, choose **More** > **Delete**.

   |image2|

#. In the displayed dialog box, confirm the deletion. You can determine whether to perform the following operations:

   -  Create a snapshot for the cluster.

      If the cluster status is normal, you can click **Create Snapshot**. In the dialog box that is displayed, enter the snapshot name and click **OK** to create a snapshot for the cluster to be deleted. After the snapshot is created, go back to the **Clusters** page and delete the cluster.

   -  Release the EIP bound to the cluster.

      If the cluster is bound with an EIP, you can click **Release the EIP bound to the cluster** to release the EIP of the cluster to be deleted.

   |image3|

#. Click **Yes**. The cluster status in the cluster list will change to **Deleting**, and the cluster deletion progress will be displayed.

   If a cluster to be deleted uses an automatically created security group that is not used by other clusters, the security group will be automatically deleted with the cluster.

   |image4|

.. |image1| image:: /_static/images/en-us_image_0000001517914165.png
.. |image2| image:: /_static/images/en-us_image_0000001466914518.png
.. |image3| image:: /_static/images/en-us_image_0000001517355565.png
.. |image4| image:: /_static/images/en-us_image_0000001518034057.png
