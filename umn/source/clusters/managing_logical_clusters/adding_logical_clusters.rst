:original_name: dws_01_7242.html

.. _dws_01_7242:

Adding Logical Clusters
=======================

#. Log in to the GaussDB(DWS) management console.

#. In the cluster list, click the name of a cluster.

#. On the **Basic Information** page, enable **Logical Cluster**.

   |image1|

   |image2|

#. Go to the **Logical Clusters** tab and click **Add Logical Cluster**.

   |image3|

#. Move the ring you want to add from the right to the left panel, enter the logical cluster name, and click **OK**.

   |image4|

.. caution::

   -  If you access the **Logical Clusters** page for the first time, the metadata of the logical cluster created at the backend is synchronized to the frontend. After the synchronization is complete, you can view information about the logical clusters at the frontend. The logical cluster name is case sensitive. For example, metadata of **lc1** and **LC1** cannot be synchronized.

.. |image1| image:: /_static/images/en-us_image_0000001400066532.png
.. |image2| image:: /_static/images/en-us_image_0000001400224888.png
.. |image3| image:: /_static/images/en-us_image_0000001450666877.png
.. |image4| image:: /_static/images/en-us_image_0000001180320351.png
