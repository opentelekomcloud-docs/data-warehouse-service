:original_name: dws_01_7246.html

.. _dws_01_7246:

Scaling Out Logical Clusters
============================

.. important::

   -  Logical clusters of version 8.1.3 and later support online scale-out.
   -  Before a scale-out, you need to enable the logical cluster mode and add a logical cluster.
   -  After scaling out or scaling in a logical cluster, you need to reconfigure the backup policy for full backup. For details, see :ref:`Configuring an Automated Snapshot Policy <dws_01_0089>`.

#. Log in to the GaussDB(DWS) management console.

#. On the displayed **Clusters** page, choose **More** > **Scale Node** > **Scale Out**.

   |image1|

#. On the **Scale Out** page, select a logical cluster or elastic cluster, choose whether to enable online scaling, and click **Next: Confirm** to confirm specifications.

   |image2|

.. |image1| image:: /_static/images/en-us_image_0000001638718388.png
.. |image2| image:: /_static/images/en-us_image_0000001687035837.png
