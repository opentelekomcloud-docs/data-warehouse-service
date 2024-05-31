:original_name: dws_01_7242.html

.. _dws_01_7242:

Adding Logical Clusters
=======================

#. Log in to the GaussDB(DWS) console. In the navigation pane, choose **Clusters** > **Dedicated Clusters**.

#. In the cluster list, click the name of the target cluster. The **Cluster Information** page is displayed.

#. Enable **Logical Clusters**. The **Logical Clusters** menu item will be displayed in the navigation pane on the left.

   |image1|

#. Go to the **Logical Clusters** tab and click **Add Logical Cluster**.

   |image2|

#. Move the ring you want to add from the right to the left panel, enter the logical cluster name, and click **OK**.

   |image3|

.. caution::

   -  If you access the **Logical Clusters** page for the first time, the metadata of the logical cluster created at the backend is synchronized to the frontend. After the synchronization is complete, you can view information about the logical clusters at the frontend. The logical cluster name is case sensitive. For example, metadata of **lc1** and **LC1** cannot be synchronized.
   -  During the conversion from a physical cluster to a logical cluster, the original resource pool configuration will be cleared. The resource pool information configured after the cluster is converted to a logical cluster will be bound to the logical cluster.

.. |image1| image:: /_static/images/en-us_image_0000001711821136.png
.. |image2| image:: /_static/images/en-us_image_0000001759420721.png
.. |image3| image:: /_static/images/en-us_image_0000001711661636.png
