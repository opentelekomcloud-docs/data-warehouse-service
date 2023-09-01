:original_name: dws_01_00083.html

.. _dws_01_00083:

Viewing DR Information
======================

#. Log in to the GaussDB(DWS) management console.

#. In the navigation pane on the left, choose **DR Tasks**.

#. In the DR list, click the name of a DR task.

   On the page that is displayed, view the following information:

   -  **DR Information**: You can view the DR ID, DR name, DR creation time, and DR status.
   -  **Production Cluster Information**: You can view the production cluster ID, cluster name, AZ, used storage capacity, cluster DR status, and the time of the latest successful DR task.
   -  **DR Cluster Information**: You can view the DR cluster ID, cluster name, AZ, used storage capacity, cluster DR status, and the time of the latest successful DR task.
   -  **DR Configurations**: You can view and modify the DR synchronization period.

   |image1|

   .. note::

      On the DR information page of a cross-region DR task, you can only view the disk and AZ information about the current cluster. To check cluster information in the other region, switch to that region first.

.. |image1| image:: /_static/images/en-us_image_0000001686950537.png
