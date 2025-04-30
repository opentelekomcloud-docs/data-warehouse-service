:original_name: dws_01_1016.html

.. _dws_01_1016:

Restoring a Table to the Original Cluster
=========================================

Scenario
--------

You can create a table from a cluster or schema snapshot to the original cluster if the table was modified or deleted by mistake.

.. important::

   -  If the current console does not support this feature, contact technical support.

   -  Only the tables stored in OBS can be used to restore data to the original cluster.
   -  Currently, only cluster- and schema-level snapshots can be used for such restoration.
   -  Restoration can be performed only if the snapshot and the cluster are both in the **Available** state.
   -  A table in a read-only cluster cannot be restored.
   -  Fine-grained restoration does not support tables in an absolute or relative tablespace.

Prerequisites
-------------

Manually enable the fine-grained snapshot.

#. Choose **Management** > **Snapshots**. Alternatively, in the cluster list, click the name of the target cluster to switch to the **Cluster Information** page. Then, click **Snapshots**.

#. Click **Create Snapshot** in the upper right corner. Alternatively, choose **More** > **Create Snapshot** in the **Operation** column.

#. Click |image1| next to **Snapshot Level** and click **Set**.

   |image2|

#. On the **Snapshot List** page, toggle the fine-grained snapshot switch.

   |image3|: enabled

   |image4|: disabled

   |image5|

   .. note::

      -  If the fine-grained snapshot is enabled, you can restore specific tables from automatic or manual snapshots.

Procedure
---------

#. Log in to the GaussDB(DWS) console.
#. In the navigation pane, choose **Management** > **Snapshot**.
#. In the **Operation** column of a snapshot, click **Restore**.
#. On the **Restore Table** page, configure the following parameters:

   -  **Database:** To restore a cluster snapshot, select a database. To restore a schema snapshot, select the database specified during backup. For details, see :ref:`Creating a Manual Snapshot of a Cluster <dws_01_0028>` and :ref:`Creating a Manual Snapshot of a Schema <dws_01_1012>`.
   -  **Source Schema**: Specify the schema of the table to be restored.
   -  **Source Table**: Specify the name of the table to be restored.
   -  **Destination Schema**: Specify the schema where the table is to be restored to.
   -  **Destination Table**: Specify the name of the new table.

   .. caution::

      -  A table name must meet the following GaussDB(DWS) database naming constraints: The table name is case sensitive can contain up to 63 characters. It must start with a letter or underscore (_). Letters, digits underscores (_) are allowed.
      -  The source table to be restored must be a table in the backup set, or the restoration will fail.
      -  If the target table already exists in the database, this table will be overwritten during restoration. Check the table name before starting restoration.

#. Confirm the information and click **Restore**.

.. |image1| image:: /_static/images/en-us_image_0000002167906512.png
.. |image2| image:: /_static/images/en-us_image_0000002203312733.png
.. |image3| image:: /_static/images/en-us_image_0000002203427205.png
.. |image4| image:: /_static/images/en-us_image_0000002167906520.png
.. |image5| image:: /_static/images/en-us_image_0000002168066216.png
