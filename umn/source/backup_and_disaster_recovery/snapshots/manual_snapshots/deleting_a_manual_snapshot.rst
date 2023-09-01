:original_name: dws_01_0030.html

.. _dws_01_0030:

Deleting a Manual Snapshot
==========================

On the **Snapshot Management** page of the GaussDB(DWS) management console, you can delete an unwanted snapshot in the **Unavailable** state or delete an available snapshot to release the storage space.

.. caution::

   Deleted snapshots cannot be recovered. Exercise caution when performing this operation.

Procedure
---------

#. Log in to the GaussDB(DWS) management console.

#. In the navigation pane, choose **Snapshots**. All snapshots are displayed by default.

#. In the **Operation** column of the snapshot that you want to delete, choose **More** > **Delete**.

   |image1|

   .. note::

      You can only delete snapshots that were manually created.

#. In the dialog box that is displayed, confirm the information and click **Yes** to delete the snapshot.

   |image2|

.. |image1| image:: /_static/images/en-us_image_0000001517913985.png
.. |image2| image:: /_static/images/en-us_image_0000001466914338.png
