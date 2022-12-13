:original_name: dws_01_7243.html

.. _dws_01_7243:

Editing Logical Clusters
========================

#. Log in to the GaussDB(DWS) management console.

#. In the cluster list, click the name of a cluster.

#. Switch to the **Logical Clusters** tab and click **Edit** in the **Operation** column of the target cluster.

   |image1|

#. Add a node to the logical cluster by moving the selected ring from the right to the left, or remove a node from the logical cluster by moving the selected ring from the left to the right, and click **OK**.

   |image2|

.. note::

   -  Nodes are added to or removed from a logical cluster by ring.
   -  At least one ring must be reserved in a logical cluster.
   -  The ring removed from the logical cluster will be added to the elastic cluster.

.. |image1| image:: /_static/images/en-us_image_0000001432193733.png
.. |image2| image:: /_static/images/en-us_image_0000001180320297.png
