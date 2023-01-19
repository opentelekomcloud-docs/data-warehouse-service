:original_name: dws_01_0021.html

.. _dws_01_0021:

Viewing Snapshot Information
============================

This section describes how to view snapshot information on the **Snapshots** page.


Viewing Snapshot Information
----------------------------

#. Log in to the GaussDB(DWS) management console.

#. In the navigation pane on the left, choose **Snapshots**.

   In the snapshot list, all snapshots are displayed by default. Click |image1| next to the snapshot name to display the snapshot details.

   |image2|

#. You can view **Snapshot Name**, **Snapshot Status**, **Cluster Name**, **Snapshot Type**, and **Snapshot Created** of snapshots.

   You can also enter a snapshot name or cluster name in the upper right corner of the snapshot list and click |image3| to search for the specified snapshot. GaussDB(DWS) supports fuzzy search.

   :ref:`Table 1 <en-us_topic_0000001180320191__table3259774163926>` describes snapshot status.

   .. _en-us_topic_0000001180320191__table3259774163926:

   .. table:: **Table 1** Snapshot status

      +-----------------+---------------------------------------------------------------+
      | Status          | Description                                                   |
      +=================+===============================================================+
      | **Available**   | Indicates that the existing snapshot works properly.          |
      +-----------------+---------------------------------------------------------------+
      | **Creating**    | Indicates that a snapshot is being created.                   |
      +-----------------+---------------------------------------------------------------+
      | **Unavailable** | Indicates that the existing snapshot cannot provide services. |
      +-----------------+---------------------------------------------------------------+

   :ref:`Table 2 <en-us_topic_0000001180320191__table875924217540>` describes the snapshot types.

   .. _en-us_topic_0000001180320191__table875924217540:

   .. table:: **Table 2** Snapshot types

      +---------------+--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
      | Type          | Description                                                                                                                                                                                                                                                  |
      +===============+==============================================================================================================================================================================================================================================================+
      | **Manual**    | Indicates the snapshot that you manually create through the GaussDB(DWS) management console or using APIs. You can delete the snapshots that are manually created.                                                                                           |
      +---------------+--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
      | **Automated** | Indicates the snapshot that is automatically created after the automated snapshot backup policy is enabled. You cannot delete the snapshots that are automatically created. The system automatically deletes the snapshots whose retention duration expires. |
      +---------------+--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+

.. |image1| image:: /_static/images/en-us_image_0000001180440325.jpg
.. |image2| image:: /_static/images/en-us_image_0000001277273485.png
.. |image3| image:: /_static/images/en-us_image_0000001134560756.jpg
