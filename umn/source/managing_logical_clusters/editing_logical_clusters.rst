:original_name: dws_01_7243.html

.. _dws_01_7243:

Editing Logical Clusters
========================

#. Log in to the GaussDB(DWS) console. In the navigation pane, choose **Clusters** > **Dedicated Clusters**.

#. In the cluster list, click the name of the target cluster. The **Cluster Information** page is displayed.

#. In the navigation pane, choose **Logical Clusters** and click **Edit** in the **Operation** column of the target cluster.

   |image1|

#. Add a node to the logical cluster by moving the selected ring from the right to the left, or remove a node from the logical cluster by moving the selected ring from the left to the right, and click **OK**.

   |image2|

#. When adding a node, select online or offline scale-out as needed.

   |image3|

.. note::

   -  Nodes are added to or removed from a logical cluster by ring.
   -  At least one ring must be reserved in a logical cluster.
   -  The ring removed from the logical cluster will be added to the elastic cluster.
   -  Logical clusters of version 8.1.3 and later support online scale-out.

.. |image1| image:: /_static/images/en-us_image_0000001711048672.png
.. |image2| image:: /_static/images/en-us_image_0000001711208184.png
.. |image3| image:: /_static/images/en-us_image_0000001758847757.png
