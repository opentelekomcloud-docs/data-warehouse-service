:original_name: dws_01_1036.html

.. _dws_01_1036:

Restoring a Table or Multiple Tables to a New Cluster
=====================================================

Scenario
--------

You can create a table from a cluster or schema snapshot to the original cluster. In case of accidental deletion or operation on data in a table during service operations, you can use this function to find the latest snapshot that contains the table data and restore it to a new cluster. Compare the data between the old and new clusters without affecting the original table data, and then restore the data as needed.

.. important::

   -  This function is supported only by clusters of version 9.1.0 or later. The system supports OBS.

   -  You can restore fine-grained snapshots of clusters from an earlier version to a new cluster of version 9.1.0, even if the versions are different.
   -  You can restore the fine-grained snapshot of the 9.1.0 cluster to a new heterogeneous cluster of version 9.1.0, even if the number of nodes and specifications of the old and new clusters are different.
   -  Only fine-grained single-table or multi-table snapshots can be restored to a new cluster.

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
#. Choose **Management** > **Snapshots**. Alternatively, in the cluster list, click the name of the target cluster to switch to the **Cluster Information** page. Then, click **Snapshots**. All snapshots are displayed by default.
#. In the **Operation** column of a snapshot, click **Restore**.
#. Set the recovery level to the table level.
#. Select the basic information about the new cluster to be restored. For details, see :ref:`Creating a GaussDB(DWS) Storage-Compute Coupled Cluster <dws_01_0019>`.

   .. note::

      -  If fine-grained heterogeneous recovery is available, you can select different node specifications and quantities for the new cluster, regardless of whether they match those of the original cluster.
      -  You need a cluster version of 9.1.0 or later to restore one or more tables to a new cluster.

#. Select a single table or multiple tables. Select a database name from the drop-down list. If you select custom database configuration, you can adjust the following configuration parameters. If you select the default configuration, the parameters will use their default values. Once you've finished configuring, choose one or more tables in the table list to restore.

   .. note::

      When you restore data to a new cluster, a new database is created. If the configuration of the new database is not the same as the snapshot database, the restoration process may fail. Before restoring, make sure to review the configuration of the original database. If it differs from the default configuration, adjust it accordingly.

   .. table:: **Table 1** Custom database parameters

      +--------------------------+--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+---------------------------------------------------------------+-----------------+
      | Parameter                | Description                                                                                                                                                                                                                                                                                                                                                  | Value Range                                                   | Default Value   |
      +==========================+==============================================================================================================================================================================================================================================================================================================================================================+===============================================================+=================+
      | Template Name            | Name of the template from which the database is created. GaussDB(DWS) creates a database by copying a database template. GaussDB(DWS) has two initial template databases **template0** and **template1** and a default user database **gaussdb**.                                                                                                            | Names of existing databases, **template0**, and **template1** | template0       |
      +--------------------------+--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+---------------------------------------------------------------+-----------------+
      | Character Encoding       | -  Encoding format used by the new database. The value can be a string (for example, **SQL_ASCII**) or an integer.                                                                                                                                                                                                                                           | Value range: **GBK**, **UTF8**, **Latin1**, and **SQL_ASCII** | SQL_ASCII       |
      |                          | -  By default, the encoding format of the template database is used. The encoding formats of the template databases **template0** and **template1** vary based on OS environments by default.                                                                                                                                                                |                                                               |                 |
      |                          |                                                                                                                                                                                                                                                                                                                                                              |                                                               |                 |
      |                          |    -  The **template1** database does not allow encoding customization. To specify encoding for a database when creating it, use **template0**.                                                                                                                                                                                                              |                                                               |                 |
      |                          |    -  To specify encoding, set **template** to **template0**.                                                                                                                                                                                                                                                                                                |                                                               |                 |
      +--------------------------+--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+---------------------------------------------------------------+-----------------+
      | Character Set Support    | Character set used by the new database. For example, this parameter can be set using **lc_collate = 'zh_CN.gbk'**. The use of this parameter affects the sort order applied to strings, for example, in queries with **ORDER BY**, as well as the order used in indexes on text columns. The default is to use the collation order of the template database. | Valid collation order                                         | C               |
      +--------------------------+--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+---------------------------------------------------------------+-----------------+
      | Character Classification | Character classification to use in the new database. For example, this parameter can be set using **lc_ctype = 'zh_CN.gbk'**. The use of this parameter affects the categorization of characters, for example, lower, upper and digit. The default is to use the character classification of the template database.                                          | Valid character classification                                | C               |
      +--------------------------+--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+---------------------------------------------------------------+-----------------+
      | Type                     | Compatible database type.                                                                                                                                                                                                                                                                                                                                    | ORA, TD, and MySQL                                            | ORA             |
      +--------------------------+--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+---------------------------------------------------------------+-----------------+

#. Click **Next: Confirm**.
#. Confirm the information and click **Restore**.

.. |image1| image:: /_static/images/en-us_image_0000002167906512.png
.. |image2| image:: /_static/images/en-us_image_0000002203312733.png
.. |image3| image:: /_static/images/en-us_image_0000002203427205.png
.. |image4| image:: /_static/images/en-us_image_0000002167906520.png
.. |image5| image:: /_static/images/en-us_image_0000002168066216.png
